---
layout: post
title:  "ANSIBLE - Install EPEL using RPM"
date:   2018-06-20 07:04:16 -0300
tags: epel ansible rpm
---
Install from RPM - Ansible can do it!

You just need a very simple, two step Ansible playbook, as the one that follows (for RHEL/CentOS 7):
{% highlight bash_session %}
- hosts: all
  gather_facts: yes
  tasks:
  - name: Install rpm
    yum:
      name: http://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
      state: present

  - name: Install epel
    yum:
      name: epel-release
      state: present
{% endhighlight %}
