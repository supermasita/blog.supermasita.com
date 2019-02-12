---
layout: post
title:  "BASH - Simple script to check a URL"
date:   2018-04-27 08:04:16 -0300
tags: bash ansible linux  computers
---

Some of your servers have issues contacting a URL from time to time? We can add a cronjob with a BASH script to keep track of it.

Lets do two things:
1. Create the script
2. Deploy it with Ansible

## "Hey, ho, lets go!"

#### This is the script, which we will saved as an Ansible template (check_url.sh.j2)

```bash
#!/bin/bash
# {{ script_dir }} will be a variable in our playbook
script_path="{{ script_dir }}"
logs_path="$script_path/logs"
log="$logs_path/check_url.log"

check_url (){
        ts=`date "+%Y%m%d-%H%M%S"`
        log_name=`echo $1 | cut -d"/" -f 3`
        log_url="$logs_path/$log_name-$ts.log"
        log_trace="$logs_path/$log_name-$ts-trace.log"

        time curl -v $1 --connect-timeout 5 -m 5 -f > $log_url 2>&1

        # if CURL fails, log that and create a new log with a TRACEROUTE
        if [ $? -ne 0 ];then
                echo "$ts;ERROR;$1" | tee -a $log
                echo $1 | cut -d"/" -f 3 | xargs traceroute -m 6 > $log_trace
                return
        fi
        # No problem? Log it
        if [ $? -eq 0 ];then
                echo "$ts;OK;$1" | tee -a $log
        fi

        # Log output should look something like this (which being a CSV, could be used in Excel, etc)
        # 20180427-1423;OK;https://blog.supermasita.com
        # 20180427-1424;ERROR;https://blog.supermasita.com
}

# You could use the following and just use a parameter from the command line:
#  check_url $1
# You would then call it from command line:
# $ bash check_url.sh https://blog.supermasita.com
check_url "https://blog.supermasita.com"
```

#### Create the playbook (check_url.yml)

```yml
- hosts: servers
  gather_facts: yes
  # vars needed for our template
  vars:
  - script_dir: "/root/check_url/"
  tasks:
  - name: Create script dir
    file:
      path: "{{ script_dir }}"
      state: directory

  - name: Create script logs dir
    file:
      path: "{{ script_dir }}/logs"
      state: directory

  - name: copy script template
    template:
      src: check_url.sh.j2
      dest: "{{ script_dir }}/check_url.sh"
      mode: 0744

  # Add a cronjob in /etc/cron.d/, that will run every minute, for ever.
  - name: create cron
    cron:
      name: check_url
      user: root
      job: "{{ script_dir }}/check_url.sh > {{ script_dir }}/logs/check_url.cron.out 2>&1"
      cron_file: kaltura-check_url

  # Debian/Ubuntu and CentOS/RHEL don't have the same name for the CRON service
  - name: restart cron (Debian)
    service:
      name: cron
      state: restarted
    when: ansible_distribution == 'Debian' or ansible_distribution == 'Ubuntu'

  - name: restart crond (RHEL)
    service:
      name: crond
      state: restarted
    when: ansible_distribution == 'CentOS' or ansible_distribution == 'Red Hat Enterprise Linux'
```
