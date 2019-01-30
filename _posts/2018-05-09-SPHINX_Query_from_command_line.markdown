---
layout: post
title:  "SPHINX - Query from command line"
date:   2018-05-09 07:04:16 -0300
tags: sphinx sql computers
---
If you know your schema, you can just query it MySQL's client.

Example:
{% highlight mysql %}
$ mysql -h 127.0.0.1 -P 9312
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 1
Server version: 2.2.1-id64-dev (r4097)
 
Copyright (c) 2000, 2013, Oracle and/or its affiliates. All rights reserved.
 
Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.
 
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
 
mysql> SELECT * FROM entry where match('@entry_id "0_q3m5gmpr"')\G;
*************************** 1. row ***************************
                id: 4337757
      int_entry_id: 1971494773
              type: 5
        media_type: 3
             views: 0
 moderation_status: 6
   length_in_msecs: 0
 access_control_id: 25
  moderation_count: 0
              rank: 0
        total_rank: 0
             plays: 0
replacement_status: 0
            source: 0
      entry_status: 2
 display_in_search: 1
partner_sort_value: 0
        created_at: 1475009362
        updated_at: 1475009393
       modified_at: 1475009362
        media_date: 0
        start_date: 0
          end_date: 0
    available_from: 1475009362
    last_played_at: 0
      str_entry_id: 0_q3m5gmpr
              name: Tutorial
1 row in set (0.00 sec)
 
ERROR:
No query specified
{% endhighlight %}
