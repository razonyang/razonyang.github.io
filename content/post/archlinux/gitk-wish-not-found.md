---
title: "gitk exec: wish: not found"
date: 2019-02-20T14:30:18+08:00
lastmod: 2019-02-20T14:30:18+08:00
draft: false
comment: true
tags: ["git", "gitk", "wish", "tk", "archlinux"]
categories: ["development"]
---

> /usr/bin/gitk: line 3: exec: wish: not found.

这是因为 `gitk` 依赖 `tk` 包，我们需要手动安装依赖：

```
$ sudo pacman -S tk
```
<!--more-->
