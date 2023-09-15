---
title: "2023-09-15-tfstate-migration"
date: 2023-09-15T16:52:54-05:00
draft: false
categories: [ daily ]
tags : [ blog]
---

migrating and syncing state between tf.io and s3.  
you can have different remotes in different branches.  if you dont care about the state at all then when you switch branches go in and `rm -rf .terraform/`  this will wipe anything that terraform had setup locally and then use the remote of the branch to do things. 
if you want to migrate from terraform.io to s3 then do this. 
do 
```
# migration
git checkout <terraform-io branch>
rm -rf .terraform/
tf init
tf plan 
# looks good no changes expected
git checkout <s3 remote branch>
tf init -migrate-state 
tf plan 
# looks good no changes
```  
there you have it your s3 remote is up to date from your latest stuff.  
then you can go in and delete the tf.io workspace and continue on with your life.

```
# syncing tf.io-current-state to s3-outdated-state
git checkout <terraform-io branch>
rm -rf .terraform/
tf init
tf plan 
# looks good no changes expected
git checkout <s3 remote branch>
tf init -migrate-state 
# here you will get a warning saying eeek  there is something already in s3 do you want to continue
# you answer yes
# you can avoid this error by manually deleting the tfstate file in the s3 bucket as well but you do you
tf plan 
# looks good no changes
``` 

profit