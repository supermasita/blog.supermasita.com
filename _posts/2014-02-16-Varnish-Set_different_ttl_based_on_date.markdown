---
layout: post
title:  "Varnish : Set different TTL based on date in URL"
date:   2014-02-16 07:04:16 -0300
categories: varnish cache proxy 
---

Your site has an "archive" where you see all of today's post and all previous ones, with an URL like "http://example.com/archive/2014/01/31" - its dynamic and it performs badly...

This is what troubled [Gilgamezh](https://twitter.com/Gilgamezh), who said to me "*I want the default TTL for today's archive (which is still being created) and a longer one for previous days (which are rarely modified)*".

Here is the solution I found: **create a header with the current date (in the same format as the URL, computed by inline C) and create another header by stripinng the URL; compare them and set the TTL**.

Code snippet
------------

{% highlight bash session %}
        C{
        #include <stdio.h>
        #include <time.h>
        }C
        
        sub vcl_recv
        {
        
                # Given a URL like "http://example.com/archive/2014/01/31"
                if (req.url ~ "^/archive" ){
        
                        # Create the "X-Today" header with the date format neeeded
                        C{
                                time_t timer;
                                char buffer[25];
                                struct tm* tm_info;
        
                                time(&timer);
                                tm_info = localtime(&timer);
        
                                strftime(buffer, 25, "%Y/%m/%d", tm_info);
                                VRT_SetHdr(sp, HDR_REQ, "\010X-Today:", buffer, vrt_magic_string_end);
        
                        }C
        
                        
                        # Create the "X-DateURL" header by striping "archive" and unwanted parameters from the req.url
                        set req.http.X-DateURL = regsub(req.url, "^/archive/", "");
                        set req.http.X-DateURL = regsub(req.http.X-DateURL, "\?.*$", "");
        
                        # Compare the headers and set the TTL for older archives - "Today" gets default TTL
                        if (req.http.X-DateURL != req.http.X-Today){
        
                                set beresp.ttl = 365d;
                                set beresp.http.X-Cacheable-TTL = beresp.ttl;
        
                        }
                }
        
        
        }
{% endhighlight %}



