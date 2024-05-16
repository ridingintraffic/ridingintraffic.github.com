---
title: "ecr and aws orgs"
date: 2024-05-14-T00:00:00-05:00
draft: true
categories: [ daily ]
tags : [ blog ]
---
We have an AWS ORG, we want to share some things between accounts in the ORG. Namely ecr and code artifact.
In order to get an ECR repository to share, be shared across multiple AWS accounts, you have many, many, many different ways that you can do this. You can do it probably 12 different ways, and maybe even more than that. But I am going to do it with AWS orgs and with kind of a permissive structure in that anyone that is a part of the org will get access to the ECR. So I will accomplish this with a repository policy that is attached to every repository. And for us, that is going to be very easy. But subsequently, we have to do it in every single repository, and that is a lot of repositories. So what we are going to do is attach the policy that says if the request is coming from someone that is a part of our org, let the request through. And you do that with an AWS org ID called out in the IAM. The technical things can be looked at in detail. But that is how AWS orgs can share ECR policies across many accounts.

AWS Code Artifact repositories shared across an AWS org. In order to share code artifact repositories among separate AWS accounts and an AWS org, one way to do it is to tell code artifact that any account inside of the org can access this. Now you need to do that in two places. You need to do that in the code artifact domain. You also need to apply the same policy to the repository itself. Each repository gets an STS and gets a caller identity and yada yada yada, some technical IAM thing that I need to paste here. At the domain level, it needs both of those permissions as well. Otherwise, it doesn't work. It needs to be in both places. So in order to get everything rolling, both must be done at the same time. But that's how that works. So just turn it on, right? Make it work.