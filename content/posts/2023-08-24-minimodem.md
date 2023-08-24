---
title: "2023-08-24-minimodem"
date: 2023-08-24T17:09:09-05:00
draft: false
categories: [ daily ]
tags : [ blog]
---

# adeventures in old technology  
so there is an application called minimodem   and it will transmit and receive data at various bps etc.   why not see what it takes to move a 1.5mb file.  cause lol  
```
base64 -i img.jpg|minimodem --tx 9600 --ascii -f 9600.wav
ffmpeg -i 9600.wav 9600.mp3
ffmpeg -i 9600.mp3 9600.wav
minimodem --rx 9600 -f 9600.wav|base64 -d >img_output9600.png
```
at 9600 bps  the file is 36 minutes long. and 120mb once in mp3 its 17mb.  
if i were to play if from one audio source to another it would take 36 minutes to move the data only on audio.  
but it worked and the file moved and converted back 
lol  why,  i dont know but it was fun.   

https://manpages.ubuntu.com/manpages/focal/man1/minimodem.1.html  