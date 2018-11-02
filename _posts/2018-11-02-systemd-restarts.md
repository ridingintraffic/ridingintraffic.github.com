---
layout: single
title: 2018-11-02 systemd-restarts
modified:
categories: blog
date: 2018-11-02T08:08:50-04:00
---

# Systemd restart policy
sometimes services die.  sometimes there is not a better option because of the situatuion that you are in, and you just need to wait it out and then restart the service...  I know it isn't ideal and that there should be better ways around having to do this but hey ¯\_(ツ)_/¯ 
```
[Service]
Type=simple
Restart=always
RestartSec=3
ExecStart=/path/to/script
``` 
In my case I needed to wait it out and restart the service 2 hours after it died. because the process was filling up a hdd and the only option was to let the hdd drain, because the downstream process is the bottle neck.   two hours of seconds equals `7200` seconds. yey