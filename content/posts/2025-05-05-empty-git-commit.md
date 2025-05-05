---
title: "empty git commit"
date: 2025-05-25T00:00:00-05:00
draft: false
categories: [ daily ]
tags : [ blog ]
---
There are many more times than i would like to admit that I have added an empty line to a file or a space to a readme in order to get a fresh commit and then push that commit up in order to get the CI pipeline to fire.  There is always an easier and better way, and this is one way of doing just that.

create a git commit that triggers a ci without having to add some BS new line somewhere.
```git commit --allow-empty -m "chore: trigger ci" ```
and thats it because I can never remember this short one.