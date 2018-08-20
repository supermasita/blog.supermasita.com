---
layout: post
title:  "ANSIBLE - Update NGINX configuration"
date:   2018-07-10 07:04:16 -0300
categories: nginx ansible configuration
---
Update the configuration and restart only if needed and valid.

Self explanatory playbook:
{% highlight bash_session %}
- hosts: nginx
  tasks:
  - name: Copy nginx.conf
    copy:
      src: nginx.conf
      dest: /etc/nginx/nginx.conf
      backup: yes
    register: nginx_conf

  - name: Test nginx.conf if "nginx.conf" has changed (failed test will stop playbook)
    command: "nginx -T -c /etc/nginx/nginx.conf"
    when: nginx_conf is changed

  - name: Restart NGINX if "nginx.conf" has changed
    service:
      name: kaltura-nginx
      state: restarted
    when: nginx_conf is changed
{% endhighlight %}

