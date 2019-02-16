---
layout: post
title:  "FFMPEG - Concatenate videos"
date:   2018-06-10 07:04:16 -0300
tags: video ffmpeg  computers
---
Ups... I shouldn't have press the record button so many times...

Given the following videos, which all have the same specs:
```bash
/tmp/video1.mp4
/tmp/video2.mp4
/tmp/video3.mp4
```
Create a file with the list of videos with the following standard:
```bash
file '/tmp/video1.mp4'
file '/tmp/video2.mp4'
file '/tmp/video3.mp4' 
```
_TIP_: if you have many files, you could do something like this
```bash
$ for f in `ls /tmp/*.mp4`; do echo "file '/tmp/$f'"; done > video-list.txt
```

Run FFMPEG with "[concat](https://trac.ffmpeg.org/wiki/Concatenate)" filter
```bash
$ ffmpeg -f concat -i video-list.txt -c copy video.concat.mp4 
```

The file _video.concat.mp4_ should have all the concatenated videos
