---
title: "Arch Linux 安装教程"
date: 2018-11-12T15:04:24+08:00
lastmod: 2018-11-12T15:04:24+08:00
draft: true
comment: true
tags: ["linux", "arch linux"]
categories: ["linux"]
---

Arch Linux 安装教程
<!--more-->

## LiveUSB

### 制作

可以通过 `dd`、`USBWriter` 等工具刻录。

### 启动

因为笔记本是 4K 屏，启动后，字非常小。可以在 GRUB 引导前，编辑 Linux 的参数：

在选择启动项时，`e` 进行编辑，在尾部加上：`video=1024x768`

## 网络连接

Arch Linux 安装过程中，网络是不可或缺的。网络连接可以通过有线网络设置即可。

##