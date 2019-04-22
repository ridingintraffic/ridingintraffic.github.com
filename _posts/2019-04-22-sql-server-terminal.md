---
layout: single
title: 2019-04-22 sql server terminal
modified:
categories: blog
date: 2019-04-22-T08:08:50-04:00
---

# sql server
I needed to connect to a sql db this morning and I didn't have a client.  Docker to the rescue! <br>
`docker run -it mysql /bin/bash` <br>
`mysql -u <myuser> -p -h <myhost> <mydatabase>` <br> 
and done. <br>
Simple easy and connected.

