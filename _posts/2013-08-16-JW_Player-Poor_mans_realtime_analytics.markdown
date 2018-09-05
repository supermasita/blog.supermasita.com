---
layout: post
title:  "JW Player : Poor man's realtime analytics"
date:   2013-03-18 07:04:16 -0300
categories: google live streaming analytics realtime javascript jwplayer
---

JW has an API that allows you to have many actions in response to the player events. Google analytics also provides a great API, is probably the best and least expensive way of tracking these events.

This is a very simple example on how to track which users are currently playing a live stream.

The GA code
-----------

Get an GA account and its code (you can skip this if you already are using it):

{% highlight html %}
	<script type="text/javascript">

	  var _gaq = _gaq || [];
	  // Set your UA number	
	  _gaq.push(['_setAccount', 'UA-******-*']);
	  // Disable the default pageview tracking - OPTIONAL
	  //_gaq.push(['_trackPageview']);

	  (function() {
		var ga = document.createElement('script'); ga.type = 'text/javascript'; ga.async = true;
		ga.src = ('https:' == document.location.protocol ? 'https://ssl' : 'http://www') + '.google-analytics.com/ga.js';
		var s = document.getElementsByTagName('script')[0]; s.parentNode.insertBefore(ga, s);
	  })();

	</script>
{% endhighlight %}

Using the JW API and pushing to GA
----------------------------------

This is our code, where we set an interval of 60000 miliseconds for a loop that checks if JW Player is in "PLAYING" state. If so, a event and a pageview are pushed to GA (you can customize this in any way you like):

{% highlight html %}
	<script type="text/javascript">
		setInterval(
			function(){
				if (jwplayer().getState()=="PLAYING"){
					_gaq.push(['_trackEvent', 'Live streaming', 'Concurrente', 'Live streaming']);
					_gaq.push(['_trackPageview', 'concurrente_supermasita']);
				}
			}
		,60000);

	</script>
{% endhighlight %}


Using GA Realtime
-----------------

In your GA panel you should see something like this:

![](http://i.imgur.com/DHN8Lsh.png)
![](http://i.imgur.com/reBlgIk.png)


Reference
---------

* <http://www.longtailvideo.com/support/jw-player/28851/javascript-api-reference/>
* <https://developers.google.com/analytics/devguides/collection/analyticsjs/>

 

