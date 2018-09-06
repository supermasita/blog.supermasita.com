---
layout: post
title:  "Show IP at login"
date:   2018-05-01 07:04:16 -0300
tags: linux 
---
Want to see the IP of the server at login prompt?

Edit */etc/issue* and add "\4" to show the IPv4 of the first interface (use "\4{eth0}" if you want "eth0" only, for ex):
{% highlight bash session %}
Ubuntu 16.04.3 LTS \n \l

IP: \4
{% endhighlight %}

_Here you can find some other cool stuff you can add: [https://www.cyberciti.biz/faq/howto-change-login-message/](https://www.cyberciti.biz/faq/howto-change-login-message/)_
