---
layout: post
title:  "ANSIBLE - Use server facts in playbooks"
date:   2018-05-30 07:04:16 -0300
categories: ansible facts playbooks automation
---
If only I could make my playbook assign half the server's RAM...

You can! Use the server's **[facts](http://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html#information-discovered-from-systems-facts)**.

Example using **ansible_memory_mb.real.total** :
{% highlight bash_session %}
- hosts: pentaho
  gather_facts: yes
  tasks:
  - name: Adjust JAVAMAXMEM in kitchen.sh to half the total RAM
    replace:
      path: /opt/pentaho/pdi/kitchen.sh
      regexp: '(\s+)JAVAMAXMEM=\"512\"$'
      replace: '\1JAVAMAXMEM="{% raw %}{{ ansible_memory_mb.real.total//2 }}{% endraw %}"'
      backup: yes
{% endhighlight %}

### Extra tip
* Get filtered facts
{% highlight bash_session %}
$ ansible example -m setup -a 'filter=*mem*'
example.supermasita.com | SUCCESS => {
    "ansible_facts": {
        "ansible_memfree_mb": 107, 
        "ansible_memory_mb": {
            "nocache": {
                "free": 1647, 
                "used": 353
            }, 
            "real": {
                "free": 107, 
                "total": 2000, 
                "used": 1893
            }, 
            "swap": {
                "cached": 0, 
                "free": 0, 
                "total": 0, 
                "used": 0
            }
        }, 
        "ansible_memtotal_mb": 2000
    }, 
    "changed": false
}
{% endhighlight %}
