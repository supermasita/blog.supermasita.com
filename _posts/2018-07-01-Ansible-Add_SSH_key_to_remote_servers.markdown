---
layout: post
title:  "Ansible - Add SSH key to remote servers"
date:   2018-07-01 07:04:16 -0300
tags: ansible ssh computers
---
Please, don't use passwords...

Assuming you have a user called "admin" which can access with password and become "root" using SUDO, create a playbook containing your SSH public key (note the "remote_user" variable):
```yaml
hosts: all
  gather_facts: yes
  remote_user: admin
  tasks:
  - name: check if key is authorized
    lineinfile:
      dest: /root/.ssh/authorized_keys
      backup: yes
      state: present
      line: "ssh-rsa AAAAxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
```

Run using the parameters "-k" (ask for password), "-b" (become another user) and "-K" (ask for SUDO password):
```
$ ansible-playbook copy_key.yml -k -b -K
SSH password: 
SUDO password[defaults to SSH password]:
```
Done! 
