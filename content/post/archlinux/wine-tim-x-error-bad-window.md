---
title: "Wine Tim X Error Bad Window"
date: 2019-02-20T17:12:50+08:00
lastmod: 2019-02-20T17:12:50+08:00
draft: true
comment: true
tags: []
categories: ["development"]
---

> X Error of failed request:  BadWindow (invalid Window parameter)
  Major opcode of failed request:  20 (X_GetProperty)
  Resource id in failed request:  0x0
  Serial number of failed request:  10
  Current serial number in output stream:  10
<!--more-->

```
$ sudo pacman -S gnome-settings-daemon
$ /usr/lib/gsd-xsettings &
```