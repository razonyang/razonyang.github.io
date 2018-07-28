---
title: "GRUB 记住上次启动项选择"
date: 2018-07-29T00:49:10+08:00
comment: true
tags: ["grub"]
categories: ["development"]
---

本文介绍如何设置 GRUB，以记住上次启动项的选择。
<!--more-->

## 修改 GRUB 配置

```
vi /etc/default/grub

GRUB_DEFAULT=saved
GRUB_SAVEDEFAULT=true
```

## 更新 GRUB

### Ubuntu

```
update-grub
```

### Gentoo

```
grub-mkconfig -o /boot/grub/grub.cfg
```
