---
layout: post
title:  "BASH - Avoid command alias"
date:   2018-08-01 07:04:16 -0300
tags: linux bash computers
---
Why does `ls` want to colour all my files!?

You probably been there: try to list a million files and `ls` wants to add colour to all of them... or `cp` is set to interactive... 

Of course, you can change the alias or `unalias` it - but this is simpler: _just escape the command_.

Simple example:
```bash
$ alias | grep ls
alias l='ls -CF'
alias la='ls -A'
alias ll='ls -lF'
alias ls='ls --color -lh'
$ ls
total 4.0K
drwxrwxr-x 2 user user 4.0K Aug 21 22:45 dir
-rwxrwxr-x 1 user user    0 Aug 21 22:45 file1
-rw-rw-r-- 1 user user    0 Aug 21 22:45 file2
$ \ls
dir  file1  file2
```

