---
layout: post
title:  "Google analytics - Track events using cURL"
date:   2018-12-01 07:04:16 -0300
tags: google analytics curl computers 
---
You don't need Javascript or even a specific library.

This example uses [cURL](https://curl.haxx.se/) from the terminal, but you can use [Requests](http://docs.python-requests.org/en/master/) in a Python script or anything else that can do a HTTP request. 

As you can see, I use many parameters in POST data (_you can also use GET_):
```bash
$ curl https://www.google-analytics.com/collect \
    -d "tid=UA-xxxxxxxxxx-2" \
    -d "t=event" \ 
    -d "ec=testCategory" \ 
    -d "ea=testAction" \ 
    -d "v=1" \ 
    -d "cid=12345678" \
    -o /dev/null
```

### Parameters
* "tid", your tracking id
* "t", the hit type
* "ec", the event category
* "ea", the event action
* "v", is version (and its always 1)
* "cid", is the client unique ID (you should create one for each of your users, if you want to track them)
* "-o /dev/null", I am outputing the result to /dev/null

### GA screenshot
![GA screenshot](https://blog.supermasita.com/assets/posts_pics/2018-12-01-GOOGLE_ANALYTICS-Track_events_using_cURL.pic01.png)


### References
* <https://developers.google.com/analytics/devguides/collection/protocol/v1/reference>
* <https://developers.google.com/analytics/devguides/collection/protocol/v1/parameters>
* <https://blog.supermasita.com/2013/03/18/JW_Player-Poor_mans_realtime_analytics.html>

### Alternatives
It is always worth mentioning that Google Analytics is not the first nor the only analytics tools. Here are three examples that are worth noting, two of which are open source and you can deploy in your network instead of using their SaaS:
* [Matomo](https://matomo.org/) - Open source
* [Simple Analitycs](https://simpleanalytics.io/) - Doesn't use cookies
* [Fathom Analytics](https://usefathom.com/) - Open source
