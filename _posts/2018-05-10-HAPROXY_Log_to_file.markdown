---
layout: post
title:  "HAPROXY - Log to file"
date:   2018-05-10 07:04:16 -0300
tags: haproxy rsyslog
---
The default HAPROXY install will will not log to a file...

You will need to modify **Rsyslog** and **HAProxy** configuration.

* Rsyslog:  Edit /etc/rsyslog.conf 
  ```
  # rsyslog v5 configuration file
   
  # For more information see /usr/share/doc/rsyslog-*/rsyslog_conf.html
  # If you experience problems, see http://www.rsyslog.com/doc/troubleshoot.html
   
  #### MODULES ####
   
  $ModLoad imuxsock # provides support for local system logging (e.g. via logger command)
  $ModLoad imklog   # provides kernel logging support (previously done by rklogd)
  #$ModLoad immark  # provides --MARK-- message capability
   
  # ENABLED THIS FOR HAPROXY LOGGING
  # Provides UDP syslog reception
  $ModLoad imudp
  $UDPServerRun 514
  $UDPServerAddress 127.0.0.1
  ```
* HAProxy: Add /etc/rsyslog.d/haproxy with the following contents
  ```
  $template Haproxy,"%msg%\n"
  local0.=info -/var/log/haproxy/haproxy.log;Haproxy
  local0.notice -/var/log/haproxy/haproxy-status.log;Haproxy
  ### keep logs in localhost ##
  local0.* ~
  ```
* After the changes, you will need to restart both HAProxy and Rsyslog.
