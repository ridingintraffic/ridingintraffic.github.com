---
layout: single
title: 2018_08_02_mac_key_repeat	
date: 2018-08-02 13:12 -0500
---

# setting up mac keyboards key repeat
with a new laptop means that you need to make all the changes to all the things.  One of the annoyances that I had was the key repeat.   Yes you can change this in the gui but why would I want to do it there when I can do it in the terminal instead...

```
# read what everything is set at
$ defaults read -g InitialKeyRepeat
$ defaults read -g KeyRepeat
# write some new values to the things.
$ defaults write -g InitialKeyRepeat -int 15
$ defaults write -g KeyRepeat -int 2
```