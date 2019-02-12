---
layout: post
title:  "Nginx - Change cache key based on URL"
date:   2018-05-07 07:04:16 -0300
tags: nginx cache proxy computers
---
I needed to have cache on "/" based on the user's IP, but based on the full URL when parameters were passed. 

This webpage provides MaxMind information: when you request "/" you will see your own, but you could use "/?ip=xxx.xxxx.xxx.xxx" to get information of any IP. So I needed an NGINX cache that was unique per IP on "/" but shared for "/?ip=xxx.xxxx.xxx.xxx".

To achieve this, you need to change the default "[proxy_cache_key](http://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_key)" configuration. This was my approach:

{% highlight bash session %}

    # Default value for all requests
    set $cache_key "$scheme$request_uri";

    # if "/" we change it
    if ($request_uri ~ "^/$"){
       set $cache_key "$scheme-$remote_addr";
    }
    
    # we change the key using the $cache_key variable
    proxy_cache_key $cache_key;
    
    # add a header for debug
    add_header X-Cache-Key $cache_key;

{% endhighlight %}

This can be improved, but gave me the cornerstone for my caching needs.
