---
date: "2017-07-09T11:08:49+08:00"
comment: true
categories: ["development", "linux"]
tags: ["fcitx", "linux"]
title: "Linux 安装 fcitx 中文输入法和搜狗输入法"
---

安装完　Linux 系统后，通常我们都需要安装中文输入法，
本文将讲述如何在 Linux 中安装 fcitx 中文输入法和搜狗输入法。
<!--more-->

# 安装

```
sudo apt install fcitx fcitx-frontend-all \
fcitx-frontend-gtk2 fcitx-frontend-gtk3 fcitx-ui-classic \
fcitx-frontend-qt4 fcitx-frontend-qt5
```

**kde-config-fcitx**

因为我使用的kde的桌面，所以需要安装 kde-config-fcitx

```
sudo apt install kde-config-fcitx
```

**安装中文输入法**

这里以谷歌拼音输入法为例

```
sudo apt install fcitx-googlepinyin
```

**搜狗输入法**

下载地址：http://pinyin.sogou.com/linux/

下载完安装即可


# 配置环境变量

```
vi ~/.xprofile
```

输入以下环境变量

```
export LC_ALL=zh_CN.utf8
export XMODIFIERS=@im=fcitx
export QT_IM_MODULE=fcitx
export GTK_IM_MODULE=fcitx
fcitx -d
```

# 配置输入法

在开始菜单，打开 **Input Method**，选择 fcitx，然后重启或注销即可。


# 移除 ibus

因为 LinuxMint 发行版自带 ibus，先将他移除

```
sudo apt remove ibus-gtk ibus-qt4
```