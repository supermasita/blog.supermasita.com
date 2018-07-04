---
layout: post
title:  "FFMPEG - Concatenate videos"
date:   2018-06-10 07:04:16 -0300
categories: video ffmpeg mp4 stitch concatenate concat
---
Ups... I shouldn't have press the record button so many times...

* Given the following videos, which all have the same specs:
{% highlight bash_session %}
/tmp/video1.mp4
/tmp/video2.mp4
/tmp/video3.mp4
{% endhighlight %}

* Create a file with the list of videos with the following standard:
{% highlight bash_session %}
file '/tmp/video1.mp4'
file '/tmp/video2.mp4'
file '/tmp/video3.mp4' 
{% endhighlight %}

_TIP_: if you have many files, you could do something like this
{% highlight bash_session %}
$ for f in `ls /tmp/*.mp4`; do echo "file '/tmp/$f'"; done > video-list.txt
{% endhighlight %}

* Run FFMPEG with "[concat](https://trac.ffmpeg.org/wiki/Concatenate)" filter
{% highlight bash_session %}
$ ffmpeg -f concat -i video-list.txt -c copy video.concat.mp4 
{% endhighlight %}

* The file _video.concat.mp4_ should have all the concatenated videos
