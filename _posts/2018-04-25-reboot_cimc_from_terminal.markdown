---
layout: post
title:  "Reboot CIMC from terminal"
date:   2018-04-25 08:04:16 -0300
categories: cisco cimc ssh
---

You can't login to the web interface of the CIMC? This might solve your issue.

1. SSH using the same user/pass you use for the web interface
```
# ssh admin@10.120.0.10
admin@10.120.0.10's password:
```
2. Change the "scope" to "cimc". Note: you can type "?" (not shown in capture) to display options
```
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
```
3. You can do a "show" just because :P
```
XXX-C220-01-01 /cimc # show
Firmware Version     Current Time             Local Time
-------------------- ------------------------ ------------------------------
3.0(3a)              Mon Jul  3 09:36:08 2017 Mon Jul  3 04:36:08 2017 CO...
```
4. Use the "reboot" command. The CIMC might take around 5 minutes to reboot and be accesible again.
```
XXX-C220-01-01 /cimc # reboot
This operation will reboot the Cisco IMC.
Continue?[y|N]y
```
