---
date: 2023-02-10T00:00:00-05:00
title: 'terraform depends without depends_on'
draft: false
categories: [ blog ]
tags : [ terraform ]
---
Terraform depends pattern without depends_on.
Or how to use hcl to leverage its interal dependancy handling, to to hard things for you.
We are going to use an example of an aws sqs queue, dlq, and queue policy all strung together. The issue that I ran in to was I wanted to create all of these using a single list of words as my seed values.  Then the issue arose around using a for_each with a dynamic resource group when terraform would need group 1 to be appied before it knew what to setup for group 2.  Terraform was not happy about this and it was known as a big ball of ugh.
Lets dive in then.

First we start with a set of locals,  which is just a set of words.
```
locals {
  list_of_things = toset(["bird", "dog", "cat", "puppy", "fish"])
}
```
Then we need to construct a group of sqs queues, policies and dlqs.
In order to create a sqs with dlq we needed to have the dlq first.  This is because of the dlq needed to be referenced in the sqs.
```
resource "aws_sqs_queue"  "list_of_things_dlq" {
  for_each = local.list_of_things

  name                      = "the_thing${each.value}_dlq"
  delay_seconds             = 0
  max_message_size          = 8192
  message_retention_seconds = 86400 # 2592000 = 30 days
  receive_wait_time_seconds = 10
}
```
Alright now we have the dlq and this is going to be the only time that we reference the local directly.
Next we are going to create the group of sqs taking in the dlq and then doing a cute little replace to clean it up.
This is going to ensure that the dlq is spun up first because the for_each here is looking at the queue name for the original loop and then feeding the name value in to the queue name with a little regex scrub, and also using the actual dlq name for the deadLetterTargetARN.
```
resource "aws_sqs_queue" "list_of_things" {
  for_each = aws_sqs_queue.list_of_things_dlq

  name                      = replace(each.value.name, "_dlq", "")
  delay_seconds             = 0
  max_message_size          = 8192  # probably want to up this ... limit is 262144
  message_retention_seconds = 86400 # 24 hours
  receive_wait_time_seconds = 10
  redrive_policy = jsonencode({
    deadLetterTargetArn = each.value.arn
    maxReceiveCount     = 4
  })
}
```

Finally we are going to create the queue policies for the dlq and the regular queue by  for_each-ing over the queues individually.  Thus setting up a nice bit of recursion to handle all this.
```
## dlq policies
resource "aws_sqs_queue_policy" "list_of_things_dlq" {
  for_each  = aws_sqs_queue.list_of_things_dlq
  queue_url = each.value.id

  policy = <<POLICY
{
  "Version": "2012-10-17",
  "Id": "${each.value.name}-policy",
  "Statement": [
    {
      "Sid": "allowgrxaccount",
      "Effect": "Allow",
      "Principal": {
         "AWS": [
            "arn:aws:iam::${data.aws_caller_identity.current.account_id}:root"
         ]
      },
      "Action": "SQS:SendMessage",
      "Resource": "${each.value.arn}"
    }
  ]
}
POLICY
}
```

```
## regular queue policy
resource "aws_sqs_queue_policy" "list_of_things" {
  for_each  = aws_sqs_queue.list_of_things
  queue_url = each.value.id

  policy = <<POLICY
{
  "Version": "2012-10-17",
  "Id": "${each.value.name}-policy",
  "Statement": [
    {
      "Sid": "allowgrxaccount",
      "Effect": "Allow",
      "Principal": {
         "AWS": [
            "arn:aws:iam::${data.aws_caller_identity.current.account_id}:root"
         ]
      },
      "Action": "SQS:SendMessage",
      "Resource": "${each.value.arn}"
    }
  ]
}
POLICY
}
```
Well, there you have it. This is a neat little bit of recursion and dependancy fun in order to spin up a tree of resources dynamically and with only a limited number of change permutations.
