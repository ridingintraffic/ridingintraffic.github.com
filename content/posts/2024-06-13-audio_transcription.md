---
title: "openai whisper"
date: 2024-06-13-T00:00:00-05:00
draft: false
categories: [ daily ]
tags : [ blog, ai, llm ]
---

Lets use some ai magic to transcribe audio in the command line.
simple enough with whisper https://github.com/openai/whisper

```whisper audiofile.m4a --language English --model medium```
and then there is the python version which is oktoo
```
import whisper

model = whisper.load_model("base")
result = model.transcribe("audio.mp3")
print(result["text"])

```
I have had pretty good results with it but i think that i like the ui better for a simple macos app called whisper transcription.   there is a paid tier  but the free one is good enough for me.
https://apps.apple.com/us/app/whisper-transcription/id1668083311?mt=12
It handles all of my audio notes just fine and it is a local thing that I can run from any laptop.
