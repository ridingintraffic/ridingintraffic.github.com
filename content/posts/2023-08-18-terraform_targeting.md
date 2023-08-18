---
title: "terraform targeting"
date: 2023-08-18T16:54:04-05:00
draft: false
categories: [ daily ]
tags : [ blog]
---
# terraform targeting   
[https://github.com/ridingintraffic/ridingintraffic.github.com/wiki/code_sre#terraform-targetingd](https://github.com/ridingintraffic/ridingintraffic.github.com/wiki/code_sre#terraform-targeting)
Yes you are supposed to never use a targeted plan and apply in terraform becuase it is the violation of the Infra as code creed.   But you know what sometimes you just gotta do a thing and get it done. Then you never want to remember your shame and as a result you go and forget how to do that thing.   so here it is 


```
 terraform  plan -target="aws_appautoscaling_scheduled_action.app_scheduled_down" -target="aws_appautoscaling_scheduled_action.app_scheduled_up" input=false -compact-warnings -out plan-dev.tfplan
```
apply  
```
terraform  apply -target="aws_appautoscaling_scheduled_action.app_scheduled_down" -target="aws_appautoscaling_scheduled_action.app_scheduled_up" 
```