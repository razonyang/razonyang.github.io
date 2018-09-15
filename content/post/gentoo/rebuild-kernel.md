---
title: "Gentoo - 重建内核"
date: 2018-09-15T19:03:49+08:00
comment: true
tags: ["gentoo", "kernel"]
categories: ["gentoo"]
---

## Rebuild Kernel

```
$ make && make modules_install
```

```
$ make install
```

## Update Boot Loader

```
$ grub-mkconfig -o /boot/grub/grub.cfg
```
<!--more-->
