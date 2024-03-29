---
date: 2023-04-27T00:00:00-05:00
title: 'python nested dictionary access'
draft: false
categories: [ daily ]
tags : [ python ]
---
Getting dictionaries back from a http endpoint usually results in a mess of objects.   Dealing with this in python can be a challenge of `ValueError` exceptions.   so what is the easiest  way to handle this?    a nested get.   if the object looks like  
```
message={"detail":{"operation":"create"}}
```
then you can access it like this 
```
message.get('detail', {}).get('operation')
```
then if you access something that doesn't exist   you will get a nice `None` back 
```
>>> print(message.get('detail', {}).get('notoperation'))
None
```
hurray safe handling of missing values