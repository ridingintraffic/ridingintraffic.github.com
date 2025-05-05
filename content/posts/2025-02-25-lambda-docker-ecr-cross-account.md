---
title: "lambda docker images running cross account"
date: 2025-02-25T00:00:00-05:00
draft: false
categories: [ daily ]
tags : [ blog ]
---
Aws lambdas can run as docker images/containers.  This can be useful because I hate dealing with layers and trying to build lambdas properly with layers.
This is all fine and good in a single AWS account world but once you start doing things in AWS orgs, then the solution is always more complicated.
Lets complicate things.
The aws setup is a single aws org that has 3 aws accounts joined to it.
The deployment setup here is to have an ecr image built and pushed on `AWS_ACCOUNT_01` and the lambda itself built and deployed on `AWS_ACCOUNT_01`, `AWS_ACCOUNT_02` and `AWS_ACCOUNT_03`
The issue that we run in to is the permissions that we need to setup on the ECR first, and then the Lambda policy.  Additionally, before you deploy the lambda with a given SHA pointed to the ECR, you should make sure that the image you are pointing terraform at already exists, because if you are telling it an image sha that doesn't exist, the aws cli will give a very obtuse and unhelpful error.
The ecr on `AWS_ACCOUNT_01` would need to look like.
```
resource "aws_ecr_repository_policy" "my_lambda_policy" {
  repository = aws_ecr_repository.my_lambda.name
  policy     = <<EOF
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowPullForOrg",
      "Effect": "Allow",
      "Principal": "*",
      "Action": [
        "ecr:BatchCheckLayerAvailability",
        "ecr:BatchGetImage",
        "ecr:DescribeImages",
        "ecr:DescribeRepositories",
        "ecr:GetDownloadUrlForLayer"
      ],
      "Condition": {
        "StringEquals": {
          "aws:PrincipalOrgID": "<my_org_id>"
        }
      }
    },
    {
  "Sid": "LambdaECRImageCrossOrgRetrievalPolicy",
  "Effect": "Allow",
  "Principal": {
    "Service": "lambda.amazonaws.com"
  },
  "Action": [
    "ecr:BatchGetImage",
    "ecr:GetDownloadUrlForLayer"
  ],
  "Condition": {
    "StringEquals": {
      "aws:SourceOrgID": "<my_org_id>"
    }
  }
},
    {
      "Sid": "LambdaAccessToECR",
      "Effect": "Allow",
      "Principal": {
        "AWS": [
          "arn:aws:iam::<AWS_account_02>:role/my_lambda",
          "arn:aws:iam::<AWS_account_03>:role/my_lambda",
          "arn:aws:iam::<AWS_account_01>:role/my_lambda",
          "arn:aws:iam::<AWS_account_02>:root",
          "arn:aws:iam::<AWS_account_03>:root",
          "arn:aws:iam::<AWS_account_01>:root"
        ]
      },
      "Action": [
        "ecr:BatchCheckLayerAvailability",
        "ecr:BatchGetImage",
        "ecr:GetDownloadUrlForLayer"
      ]
    }
  ]
}
EOF
}

```

The lambda iam policy should look something like this.
You could probably roll these two policies into a single policy, but I did not do that.

```
resource "aws_iam_policy" "lambda_ecr_policy" {
    name        = "LambdaECRPolicy"
    description = "Allows Lambda to pull images from ECR"

    policy = <<EOF
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Action": [
          "ecr:BatchGetImage",
          "ecr:GetDownloadUrlForLayer"
        ],
        "Resource": "arn:aws:ecr:us-east-1:<AWS_account_01>:repository/my_lambda"
      },
      {
        "Effect": "Allow",
        "Action": "ecr:GetAuthorizationToken",
        "Resource": "*"
      }
    ]
  }
  EOF
  }
  ```

  Finally the lambda will be deployed in "aws_account_02"

  ```

resource "aws_iam_policy" "my_lambda_iam_policy" {
    name        = "my_lambda_policy_${terraform.workspace}"
    description = "Allow the Lambda function to write logs and access necessary resources"

    policy = <<EOF
  {
      "Version": "2012-10-17",
      "Statement": [
          {
              "Sid": "ECRPull",
              "Effect": "Allow",
              "Action": [
                  "ecr:BatchGetImage",
                  "ecr:GetDownloadUrlForLayer",
                  "ecr:GetRepositoryPolicy"
              ],
             "Resource": "arn:aws:ecr:us-east-1:<AWS_account_01>:repository/my_lambda"
          },
          {
              "Sid": "ECRAuth",
              "Effect": "Allow",
              "Action": "ecr:GetAuthorizationToken",
              "Resource": "*"
          }
      ]
  }
  EOF
  }
  ```