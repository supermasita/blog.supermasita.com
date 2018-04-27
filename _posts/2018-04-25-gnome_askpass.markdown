---
layout: post
title:  "GIT - Fix gnome-ssh-askpass error"
date:   2018-04-25 07:04:16 -0300
categories: linux bash beginneri git
---

Ups! "(gnome-ssh-askpass:14185): Gtk-WARNING **: cannot open display"

You may get that error when trying to push with GIT:

```
$ git push origin master
(gnome-ssh-askpass:14185): Gtk-WARNING **: cannot open display:
```
GIT is trying to open the GNOME password dialog... which is not available through terminal.

To fix it, just change the GIT_ASKPASS variable:
```
$ export GIT_ASKPASS=""
$ git push origin master
Password:
```
