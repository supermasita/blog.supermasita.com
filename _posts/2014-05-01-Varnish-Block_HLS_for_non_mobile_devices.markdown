---
layout: post
title:  "Varnish - Block HLS for non mobile devices"
date:   2014-05-01 07:04:16 -0300
tags: varnish cache proxy wowza streaming  computers
---

A tale of hotlinking and no money to develop tokenization.

Some sites were hotlinking our live streaming by using HLS using a Flash video player (like JW6). We served it to desktop players with RTMP (protected by "Domain lock") and HLS was only used by IOS. 

After contacting Wowza support, the only solution we were offered was that we develop a service with a token, in order to validate every player. The solution was pretty standard, but there was no chance we could develop this ($) and we couldn't disable IOS support.

**My workaround**: installing Varnish as a proxy for port 80 and HLS (RTMP and RTSP wouldn't be affected as they use other ports).

* Install Varnish and set it to use port 80.
* Enable Wowza HLS over port 8080 (or other) in "conf/VHost.xml":
  ```
  <Root>
   <VHost>
     <HostPortList>
       <HostPort>
          <ProcessorCount>16</ProcessorCount>
          <IpAddress>*</IpAddress>
          <!-- Separate multiple ports with commas -->
          <!-- 80: HTTP, RTMPT -->
          <!-- 554: RTSP -->
          <!--<Port>80,554,1935</Port>-->
          <Port>8080,554,1935</Port>
  ```
* Create a VCL with rules similar to these (adjust according to your configuration):
  ```
  backend default {
    .host = "127.0.0.1";
    .port = "8080";
  }
  acl mynetworks
  {
          "localhost";
  	"xxx.xxx.xxx.xxxx"/xx; 
  }
  sub vcl_recv {
  
  	if (
  		# Is this request coming from your networks?                
  		!client.ip ~ mynetworks
  
  		# Is it a mobile user agent? (extended as needed)
  
                  && !req.http.User-Agent ~ "(?i)(iphone|ipad)"
  
                  # POST needed by RTMPT - This is handled afterwards by Wowza's "Domain Lock"
                  && req.request != "POST"
  	)
  	{
  		error 403 "Forbidden";
  	}
  	else {
  		if ( 
  			# Is this coming from one my sites?
  			req.http.referer ~ "(mysite1.com|mysite2.com)"		
  
  			# Does it have an empty referer? This could be to an IOS player doing a GET over 
  			# the stream URL, and that's fine for us.
  			|| req.http.referer ~ "^$"
  		)
  		{
  			# If connection is legit, then we use PIPE in order to establish a TCP proxy
  			return (pipe);
  		}
  		else {
  			error 403 "Forbidden";
  		}
  
  	}
  	
  	error 403 "Forbidden";
  
  }
  ```
