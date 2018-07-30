---
title: "Linux 小键盘开机自启"
date: 2018-07-30T22:05:32+08:00
comment: true
tags: ["linux", "numlock", "gentoo"]
categories: ["development"]
---

笔记本每次启动后，都要手动开启小键盘，不免有些繁琐，如何让它自动开启呢？
其实方式有很多，而本文的主角是 `numlockx`。
<!--more-->


## 安装 numlockx

### Ubuntu

```
$ apt install numlockx
```

### Gentoo

```
$ emerge --ask numlockx
```

## 配置 startx

```
$ vi ~/.xinitrc

numlockx &
```

> 注意放在 `exec` 之前。
