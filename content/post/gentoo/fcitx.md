---
title: "Gentoo 安装 Fcitx 中文输入法"
nate: 2018-09-24T10:57:23+08:00
lastmod: 2018-09-24T12:06:23+08:00
comment: true
tags: ["Gentoo", "fcitx"]
categories: ["gentoo"]
---

安装 Gentoo 系统后，输入法基本是最重要的软件之一了，本篇将介绍在 Gentoo 下如何安装 Fcitx 输入法。
<!--more-->

## USE flags

```
$ vi /etc/portage/package.use/fcitx

app-i18n/fcitx gtk2 gtk3 qt4
```

这里主要添加 **gtk2**, **gtk3** 和 **qt4** 输入法模块。

> 其他 USE 标记可以看官网教程 https://wiki.gentoo.org/wiki/Fcitx


## 安装

然后我们安装 fictx 和 google 拼音输入法

```
$ emerge --ask app-i18n/fcitx app-i18n/fcitx-googlepinyin
```


## 环境变量

```
$ vi ~/.xinitrc

eval "$(dbus-launch --sh-syntax --exit-with-session)"

export XMODIFIERS="@im=fcitx"
export QT_IM_MODULE=fcitx
export GTK_IM_MODULE=fcitx
```

> 如果你使用登录管理器时（如 GDM、KDM、LightDM...），你需要在将上述环境变量写入到 `~/.xprofile`，而非 `~/.xinitrc`


## 问题排查

如果 Fcitx 无法正常使用，我们可以通过以下命令进行问题检测：

```
$ fcitx-diagnose
```

笔者在安装后，在部分程序无法切换输入法，用上述命令检查后，发现缺少 Fcitx 的 QT 的输入法模块，安装以下包即可解决：

```
$ emerge --ask app-i18n/fcitx-qt5
```
