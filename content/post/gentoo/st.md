---
title: "Gentoo 安装 ST"
date: 2018-07-30T22:52:43+08:00
comment: true
tags: ["gentoo", "st"]
categories: ["development"]
---

说起终端，笔者比较喜欢 ST，原因很明显 - Simple Terminal。
本文将介绍如何在 Gentoo 下 ST 的安装，配置，以及补丁的使用。
<!--more-->

## 配置

首先开启 `saveconfig`，这样可以保存我们自定义的配置文件了。

```
$ euse --enable savedconfig
```


## 安装

```
$ emerge --ask =x11-terms/st-0.8
```


## 修改字体

### 安装字体

笔者比较喜欢 Source Code Pro，所以先安装下字体：

```
$ emerge --ask media-fonts/source-pro
```

### 修改配置

```
$ vi /etc/portage/savedconfig/x11-terms/st-0.8

static char *font = "Source Code Pro:pixelsize=16:antialias=true:autohint=true";
```

修改后重新 `emerge` 即可。


## 补丁

默认安装的 ST 是无法翻页的...这对于笔者来说，是无法“容忍”的！

> 更多的补丁可以在以下网址找到：https://st.suckless.org/patches

那么下面笔者以安装翻页补丁为例，介绍 Gentoo 下应该如何安装 ST 补丁。

首先创建补丁目录（注意修改为你本地的版本），并下载相应的版本补丁：

```
$ mkdir -p /etc/portage/patches/x11-terms/st-0.8
$ cd /etc/portage/patches/x11-terms/st-0.8
$ wget https://st.suckless.org/patches/scrollback/st-scrollback-0.8.diff
$ wget https://st.suckless.org/patches/scrollback/st-scrollback-mouse-0.8.diff
```

然后重新 `emerge` 即可使用 `Shift` + `Page Up`/`Page Down`/`MouseWheel`进行上下翻页。
