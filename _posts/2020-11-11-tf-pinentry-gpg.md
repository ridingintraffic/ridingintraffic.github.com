---
layout: single
title: '2020-11-11 terraform destroy gpg pinentry'
modified:
categories: daily
excerpt:
tags : [  ]
image:
  feature:
---
# pinentry is broken on my mac?!
try this first
```
# Kill gpg-agent
killall gpg-agent

# Run gpg-agent in daemon mode
gpg-agent --daemon
```

# Universal handoff - clipboard mac/IOS
There is a feature in mac land that allows you to share clipboard between phone and computer if they are both on the same wifi along with the same apple account.  This can be super useful for a variety of applications.  
```
Here's how to turn on Handoff on your devices:

    On your Mac: Choose Apple () menu > System Preferences, then click General. Select “Allow Handoff between this Mac and your iCloud devices.”
    On your iPhone, iPad, and iPod touch: Go to Settings > General > Handoff, then turn on Handoff.

```  
[https://support.apple.com/en-us/HT209460](https://support.apple.com/en-us/HT209460)

