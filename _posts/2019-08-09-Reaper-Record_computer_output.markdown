---
layout: post
title:  "Reaper - Record computer output"
date:   2019-08-08 07:04:16 -0300
tags: audio recording reaper wasapi windows asio4all
---
Use Windows built in [WASAPI](https://en.wikipedia.org/wiki/Technical_features_new_to_Windows_Vista#Audio_stack_architecture) to capture audio output. Another app, your browser (ex: Youtube) - if you can hear it, you can capture it.

I'm using [Reaper](https://www.reaper.fm/) in this example, but the same should work with any other similar software.

* In **"Reaper preferences > Audio device settings > Audio system"** , select **"WASAPI (Windows 7/8/10/Vista)"**. For **"Mode"** use **"Shared loopback (CAUTION)"**.

![](https://blog.supermasita.com/assets/posts_pics/2019-08-09-Reaper-Record_computer_output.pic01.jpg)

* Create a track as usual. In my case, I'm capturing a stereo output:

![](https://blog.supermasita.com/assets/posts_pics/2019-08-09-Reaper-Record_computer_output.pic02.jpg)

* Hit 'record' and start capturing! 

![](https://blog.supermasita.com/assets/posts_pics/2019-08-09-Reaper-Record_computer_output.pic04.jpg)


### Side note: ASIO4ALL
Before WASAPI appeared (ex: Windows XP), you would probably use [ASIO4ALL](https://en.wikipedia.org/wiki/Audio_Stream_Input/Output) to achieve low latency, if you did not have an audio interface with ASIO. You can achieve the same low latency with WASAPI - no need to install anything.

### Keep reading
* [WASAPI in Wikipedia](https://en.wikipedia.org/wiki/Technical_features_new_to_Windows_Vista#Audio_stack_architecture)
* <https://www.reaper.fm/>
* <http://www.asio4all.org/>
