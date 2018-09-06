---
layout: post
title:  "ANSIBLE - Add SSH key to remote servers"
date:   2018-07-01 07:04:16 -0300
tags: ansible ssh
---
Please, don't use passwords...

Assuming you have a user called "admin" which can access with password and become "root" using SUDO

* Create a playbook containing your SSH public key. Note the "remote_user" variable.
{% highlight bash_session %}
hosts: all
  gather_facts: yes
  remote_user: admin
  tasks:
  - name: check if key is authorized
    lineinfile:
      dest: /root/.ssh/authorized_keys
      backup: yes
      state: present
      line: "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCb0i6nWnYTC13YTi/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
{% endhighlight %}

* Run using the parameters: "-k", ask for password; "-b", become another user; "-K", ask for SUDO password.
{% highlight bash_session %}
$ ansible-playbook copy_key.yml -k -b -K
SSH password: 
SUDO password[defaults to SSH password]: 
{% endhighlight %}
