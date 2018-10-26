---
title: "Gentoo 安装教程 - 安装基本系统"
date: 2018-09-01T22:51:41+08:00
comment: true
tags: ["gentoo"]
categories: ["gentoo"]
---

在上一篇教程 [Gentoo 安装教程 - 安装文件](/post/gentoo/installation-stage) 中，我们已经准备好 Gentoo 系统需要的安装文件了，
下面我们接着安装 Gentoo 的基本系统。
<!--more-->

> 注意！在进行本教程步骤之前，先确保已经挂载好相应的分区。

## 选择镜像

为了更快地安装系统或者软件包，我们需要先选择一个相较快的镜像：

```
$ mirrorselect -i -o >> /mnt/gentoo/etc/portage/make.conf
```

> 如果 `mirrorselect: command not found`，你则需要 `emerge --ask app-portage/mirrorselect`

这里笔者选择网易的镜像（空格 + 回车），貌似国内就只有网易的镜像。


## Gentoo ebuild 仓库

```
$ mkdir /mnt/gentoo/etc/portage/repos.conf
$ cp /mnt/gentoo/usr/share/portage/config/repos.conf /mnt/gentoo/etc/portage/repos.conf/gentoo.conf
```

## 复制 DNS 信息

```
$ cp -L /etc/resolv.conf /mnt/gentoo/etc/
```

## 挂载必要的文件系统

```
$ mount -t proc /proc /mnt/gentoo/proc
$ mount --rbind /sys /mnt/gentoo/sys
$ mount --make-rslave /mnt/gentoo/sys
$ mount --rbind /dev /mnt/gentoo/dev
$ mount --make-rslave /mnt/gentoo/dev
```

## chroot

```
$ chroot /mnt/gentoo /bin/bash
$ source /etc/profile
$ export PS1="(chroot) $PS1"
```

## 挂载 boot 分区

```
mount /dev/sdc2 /boot
```

> 由于笔者在上一篇文章已经挂载了，此步骤略过。


## 配置 Portage

### 更新 ebuild 数据库快照

```
$ emerge-webrsync
```

### 选择需要的 profile

首先列出可选的 profiles：

```
$ eselect profile list
  ...
  [12]  default/linux/amd64/17.0 (stable)
  [13]  default/linux/amd64/17.0/selinux (stable)
  [14]  default/linux/amd64/17.0/hardened (stable)
  [15]  default/linux/amd64/17.0/hardened/selinux (stable)
  [16]  default/linux/amd64/17.0/desktop (stable)
  [17]  default/linux/amd64/17.0/desktop/gnome (stable)
  [18]  default/linux/amd64/17.0/desktop/gnome/systemd (stable)
  [19]  default/linux/amd64/17.0/desktop/plasma (stable) *
  [20]  default/linux/amd64/17.0/desktop/plasma/systemd (stable)
  ...
```

为避免篇幅过长，且该列表随时间变化，笔者就不粘贴全部的 profiles 了。
那我们该怎么选择正确的 profile 呢？

首先，每个 profile 后面都有 `stable` 或 `dev` 的标志，我们自然是要选择 `stable` 的啦。

然后看自身的需求，如果要安装桌面版，则选择带有 `desktop` 字样的，又或者可以直接选 `desktop/plasma` 或者 `desktop/gnome`。

此外 Gentoo 默认的 **init program** 是 `openrc`，如果希望使用 `systemd`，需要选择带有 `systemd` 字样的 profile。

> profile 条目后带有 `*` 号的，表明该 profile 为当前系统的 profile。

由于笔者偏爱 KDE Plasma 桌面，所以笔者选择的是：

```
$ eselect profile set default/linux/amd64/13.0/desktop/plasma
```

> 也可以直接输入 profile 编号：`eselect profile set 19`

### 更新 @world 集合

选择新的 profile 之后，我们需要更新系统的 `@world set`

```
$ emerge --ask --update --deep --newuse @world
```

> 此时，我们可以泡杯咖啡，稍作休息，因为这一步骤真的很耗时间，如果选择了带有 `desktop` 和 `systemd` 的 profile 则会占用更长的安装时间。笔者在此步骤大概花了两三个小时。


## 配置时区

```
$ echo "Asia/Shanghai" > /etc/timezone
$ emerge --config sys-libs/timezone-data
```

## 配置地区

```
$ nano -w /etc/locale.gen

en_US ISO-8859-1
en_US.UTF-8 UTF-8
zh_CN GBK
zh_CN.UTF-8 UTF-8
```

生成地区

```
$ locale-gen
```

显示可选地区

```
$ eselect locale list
Available targets for the LANG variable:
  [1]   C
  [2]   en_US
  [3]   en_US.iso88591
  [4]   en_US.utf8
  [5]   POSIX
  [6]   zh_CN
  [7]   zh_CN.gbk
  [8]   zh_CN.utf8
```

选择地区

```
$ eselect locale set 4
```

> 这里最好选择英文的。

## 重新加载环境

```
$ env-update && source /etc/profile && export PS1="(chroot) $PS1"
```

完成基本系统的安装后，我们接下来就要[配置 Linux 内核](/post/gentoo/installation-kernel)了。
