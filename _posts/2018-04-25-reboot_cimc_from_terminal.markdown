---
layout: post
title:  "Reboot CIMC from terminal"
date:   2018-04-25 08:04:16 -0300
categories: cisco cimc ssh
---

You can't login to the web interface of the CIMC? This might solve your issue.

* SSH using the same user/pass you use for the web interface
{% highlight bash session %}
# ssh admin@10.120.0.10
admin@10.120.0.10's password:
{% endhighlight %}
* Change the "scope" to "cimc". Note: you can type "?" (not shown in capture) to display options
{% highlight bash session %}
XXX-C220-01-01# scope cimc
XXX-C220-01-01 /cimc #
  cleanup
  commit           Batch commit changes
  connect          Connect to other console
  discard          Discard all changes
  exit             Exit from command interpreter
  factory-default  Reset the Cisco IMC to factory default
  reboot           Reboot the Cisco IMC
  scope            Changes the current mode
  set              Set property values
  show             Show system information
  timezone-select  Select timezone interactively
  top              Go to the top mode
{% endhighlight %}
* Use the "reboot" command. The CIMC might take around 5 minutes to reboot and be accesible again.
{% highlight bash session %}
XXX-C220-01-01 /cimc # reboot
This operation will reboot the Cisco IMC.
Continue?[y|N]y
{% endhighlight %}
