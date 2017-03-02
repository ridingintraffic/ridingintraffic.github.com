---
layout: single
title: 2017-03-02 docker hack
modified:
categories: blog
excerpt:
tags : [docker, bash, j]
image:
  feature:
date: 2017-03-02T08:08:50-04:00


---
# docker reboot
Every once and a while for prem docker installation a reboot is needed.  There are some tools out there that can most likely do this, but today a quick and dirty bash script solved the problem.

Step one dump all the running container IDS, today there were about 23 containers running.
`docker ps -q >>ids`

Next reboot `sudo reboot` 
Finally the bash script to quickly spin them all back up. It reads the source file that was created in the earlier step and does a simple `docker start <id> `  and done.

```
#!/bin/bash
input="ids"
while IFS= read -r var
do
  docker start "$var"
done < "$input"
```
