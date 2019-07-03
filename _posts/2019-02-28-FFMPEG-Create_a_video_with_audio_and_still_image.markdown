---
layout: post
title:  "FFMPEG - Create a video with audio and still image"
date:   2019-02-28 07:04:16 -0300
tags: ffmpeg youtube audio computers 
---
Youtube, I'm sending you another "full album".

The following parameters will use "still_image.png" and merge it with "audio.wav" into the video "audio_and_image-output.mp4": 
```
$ ffmpeg -loop 1 -framerate 2 -i still_image.png -i audio.wav \
  -c:v libx264 -preset medium -tune stillimage -crf 18 -c:a aac \
  -ab 320k -strict -2 -shortest -pix_fmt yuv420p audio_and_image-output.mp4
```
Parameters
* "-loop 1", infinite loop of the image.
* "-framerate 2", just two FPS (some players may complain and you might need to go higher). 
* "-c:v libx264", convert video to [x264](https://trac.ffmpeg.org/wiki/Encode/H.264).
* "-preset medium", use the "Medium" preset of x264. 
* "-tune stillimage", tune the process for a still image.
* "-crf 18", value for [Constant Rate Factor](https://trac.ffmpeg.org/wiki/Encode/H.264#crf). 
* "-c:a aac", convert audio to [AAC](https://en.wikipedia.org/wiki/Advanced_Audio_Coding).
* "-ab 320k", use 320Kbps audio. 
* "-strict -2", to allow some of the non standard things we are doing here.  
* "-shortest", finish encoding when the shortest input stream ends.
* "-pix_fmt yuv420p", use YUV planar color space with 4:2:0 chroma subsampling (needed by some older players)

The input audio was a 24/44 WAV and the input PNG had 1080p resolution. These parameters created a video that complied with Youtube's best practices and was uploaded to that service. Tune the parameters to match your needs.

That's it!

### Keep reading
* <https://trac.ffmpeg.org/wiki/Encode/H.264>
* An old but **great** six part introduction to FFMPEG: "[The Swiss Army Knife of Internet Streaming](https://sonnati.wordpress.com/2011/07/11/ffmpeg-the-swiss-army-knife-of-internet-streaming-part-i/)"
