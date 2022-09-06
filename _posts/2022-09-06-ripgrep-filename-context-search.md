---
layout: single
title: '2022-09-06 ripgrep file context'
modified:
categories: daily
excerpt:
tags : [ devops, tools ]
image:
  feature:
---
# rip grep tip
I use ripgrep all the time and often times there are little things that can really help you find what you are serarching for.   This is one of them.

I want to find a string in a specific filename that exists in many folders.  Then I want to display those resutls with some context.

```
 rg '.wait:' -g '.gitlab-ci.yml' --context 5
```
The string that I am searching for is `.wait:`
The filename that I want to search in is `.gitlab-ci.yml`
I want to display 5 lines before and 5 lines after what I found.
