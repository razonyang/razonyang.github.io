---
title: "Gentoo 安装教程 - 引导加载程序"
date: 2018-09-02T11:09:00+08:00
comment: true
tags: ["gentoo"]
categories: ["gentoo"]
---

安装好 Gentoo 系统后，我们还需要一个引导加载程序引导 Linux 系统的启动，本文选择的是个 GRUB2 作为引导加载程序。
<!--more-->

## GRUB2

### 安装

```
$ echo 'GRUB_PLATFORMS="efi-64"' >> /etc/portage/make.conf
```

由于笔者是双系统，需要安装 **os-prober**

```
$ emerge --ask os-prober sys-boot/grub:2
```

### 配置

```
$ grub-install --target=x86_64-efi --efi-directory=/boot --removable
```

### 加载 Windows EFI 分区

```
$ mkdir /mnt/windows
$ mount /dev/sda2 /mnt/windows
```

### 再次配置

```
$ grub-mkconfig -o /boot/grub/grub.cfg
```

自此 Gentoo 已安装成功，我们马上重启进入我们的新系统吧。
