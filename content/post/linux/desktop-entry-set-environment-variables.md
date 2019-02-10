---
title: "Linux 桌面快捷方式设置环境变量"
date: 2019-02-10T22:16:33+08:00
lastmod: 2019-02-10T22:16:33+08:00
draft: false
comment: true
tags: ["linux", "desktop"]
categories: ["development"]
---

最近在 ArchLinux 上安装了网易云音乐，但是发现在 4k 屏下，相较于其他程序，显示过小。
于是尝试命令行设置全局变量进行两倍放大：

```
QT_SCALE_FACTOR=2 netease-cloud-music
```

经过尝试，`QT_SCALE_FACTOR` 可以解决问题，于是乎修改相应的桌面快捷方式：

```
Exec=env QT_SCALE_FACTOR=2 netease-cloud-music
```

> 注意其中的 `env` 指令。

当然我们也可以将变量放在 `~/.xprofile` 之类的地方，但是鉴于其他程序并没有这样的问题，所以笔者仅修改了相应的桌面快捷方式，避免影响其他软件。
<!--more-->
