---
title: "Gentoo 安装教程 - 配置系统"
date: 2018-09-02T10:29:16+08:00
comment: true
tags: ["gentoo"]
categories: ["gentoo"]
---

在完成 Linux 内核配置后，我们接下来需要配置系统，比如 **fstab**，网络，**root** 密码等等。
<!--more-->

## fstab

### 查看块设备的信息

```
$ blkid | grep sdc
/dev/sdc1: UUID="BE70-895F" TYPE="vfat" PARTLABEL="grub" PARTUUID="0e979008-e74a-4738-8397-00f7724af7d4"
/dev/sdc2: UUID="94ED-6E3C" TYPE="vfat" PARTLABEL="boot" PARTUUID="c96e3e4e-c52b-4037-a244-22f1fac76c59"
/dev/sdc3: UUID="c01f6894-d734-4db5-8731-24e6a0ad092c" TYPE="swap" PARTLABEL="swap" PARTUUID="2d04b9d2-ab67-4b80-9736-e423d8385f8a"
/dev/sdc4: UUID="546c9107-f9df-4cae-91e1-180809efc703" TYPE="ext4" PARTLABEL="rootfs" PARTUUID="e5274b71-0856-4b22-a19d-bb176602fa34"
/dev/sdc5: UUID="1c3b85e6-b45b-4c82-82ef-7acbb7645177" TYPE="ext4" PARTLABEL="home" PARTUUID="9355a8b6-cc34-4e75-acd4-7d275af52384"
```

### 配置 fstab

```
$ nano -w /etc/fstab

PARTUUID=c96e3e4e-c52b-4037-a244-22f1fac76c59   /boot        vfat    defaults,noatime     0 2
PARTUUID=2d04b9d2-ab67-4b80-9736-e423d8385f8a   none         swap    sw                   0 0
PARTUUID=e5274b71-0856-4b22-a19d-bb176602fa34   /            ext4    noatime              0 1
PARTUUID=9355a8b6-cc34-4e75-acd4-7d275af52384   /home        ext4    noatime              0 2
```

这里的配置只是个例子，**fstab** 的详细配置教程不在本文范畴，后续有空笔者在详述，或者读者自行搜索。

> 注意！**boot** 分区类型确保是 `vfat`。

## 网络

对于网络，笔者使用的是 [NetworkManager](https://wiki.gentoo.org/wiki/NetworkManager)。

```
$ euse -E networkmanager
$ emerge --ask --changed-use --deep @world
$ emerge --ask net-misc/networkmanager
```

> `euse` 命令属于 `app-portage/gentoolkit` 包

此外我们也可以安装一些 GUI 工具，如 **kde-plasma/plasma-nm**，**gnome-extra/nm-applet**

### 开机自启

openrc：

```
$ rc-update add NetworkManager default
```

systemd：

```
$ systemctl enable NetworkManager
```


## root 密码

```
$ passwd
```

## 时钟

因为笔者是 Windows 和 Gentoo 双系统，需要修改时钟配置：

```
$ nano -w /etc/conf.d/hwclock

clock="local"
```

## 帐号

因为用 **root** 帐号工作是一个十分危险的事情，我们需要建立一个普通用户。

```
$ useradd -m -G users,wheel,audio,video,usb -s /bin/bash razon
$ passwd razon
```
- `G` - groups，其中 wheel 必须
- `s` - bash，也可以指定为其他，如：/bin/zsh，不过需要额外安装 `emerge --ask zsh`

## 安装工具

### 系统日志

```
$ emerge --ask app-admin/sysklogd
$ rc-update add sysklogd default
```

### cron 守护进程

```
$ emerge --ask sys-process/cronie
$ rc-update add cronie default
```

### 文件索引

```
$ emerge --ask sys-apps/mlocate
```

### vi

```
$ emerge --ask vim
```

最后，我们[配置引导加载程序](/post/gentoo/installation-bootloader)。
