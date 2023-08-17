---
date: 2023-08-14
title: 'boost audio on video file'
draft: false
categories: [ daily ]
tags : [ blog]

Was the audio too quiet on something that you recorded?  
Are you lazy and you don't want to drop in the file to a timeline editor just so you can gain boost?  
most laptops will have ffmpeg on them, and if your mac doesnt then a simple `brew install ffmpeg`  [https://formulae.brew.sh/formula/ffmpeg](https://formulae.brew.sh/formula/ffmpeg)

```
ffmpeg -i "your_input_file.mp4"  -vcodec copy -filter:a "volume=15dB" "your_output_file.mp4"
```
there you have it  short and simple now your video file is louder.