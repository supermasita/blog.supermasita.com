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

First, create a file with the list of videos with the following standard:
```bash
file '/tmp/video1.mp4'
file '/tmp/video2.mp4'
file '/tmp/video3.mp4' 
```

Second, run FFMPEG with "[concat](https://trac.ffmpeg.org/wiki/Concatenate)" filter:
```bash
$ ffmpeg -f concat -i video-list.txt -c copy video.concat.mp4 
```

Finally, **the file "video.concat.mp4" will have all the concatenated videos**.


#### TIPS 
* If you have many files to list, you could do something like this:
```bash
$ for f in `ls /tmp/*.mp4`; do echo "file '/tmp/$f'"; done > video-list.txt
```
* If you wish to remove the metadata from the output file, use "-map_metadata -1"
* Keep in mind that you can also do this for audio files. For example with OGG files:
```bash
ffmpeg -f concat -i list.txt -map_metadata -1 -c copy audio.concat.ogg
```
* If you try to concat FLAC, the output file will have audio, but the metadata will be broken (the total time will not be added, for example). You will need to specify the codec instead of using "copy":
```
ffmpeg -f concat -i list.txt -map_metadata -1 -c:a flac audio.concat.flac
```


### Keep reading
* <https://trac.ffmpeg.org/wiki/Concatenate>
