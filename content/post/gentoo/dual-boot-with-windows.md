---
title: "Gentoo 和 Windows 双系统启动"
date: 2018-07-29T00:14:55+08:00
comment: true
tags: ["gentoo", "grub", "windows", "双系统"]
categories: ["development"]
---

笔者平常使用 Gentoo 办公，但是在娱乐游戏方面，Windows 系统必不可少。
但是在安装完 Gentoo 后，并没有 Windows 系统的启动项。
本文将介绍如何设别和添加 Windows 系统的启动项。
<!--more-->

在开始之前，先说明下概况：

- Bootloader - GRUB
- Gentoo 和 Windows 是分别位于不同硬盘，Windows - `/dev/sda`，Gentoo - `/dev/sdb`

## 安装 os-prober

```
emerge --ask sys-boot/os-prober
```

此软件可以识别操作系统，并将他们添加到引导装载程序。


## 挂载 Windows 分区

由于笔者电脑的 Windows 和 Gentoo 不处于相同的硬盘分区，所以需要挂载 Windows 的启动分区，os-prober 才能识别并添加它。

我们可以用 `fdisk -l` 查看 Windows 的启动分区是哪个，比如笔者的：

```
$ fdisk -l

...
/dev/sda2    1024000   1228799    204800  100M EFI System
...
```

可以看到笔者的 Windows 启动分区为 `/dev/sda2`，紧接着挂载这个分区：

```
mkdir /mnt/windows
mount /dev/sda2 /mnt/windows
```


## 更新 GRUB 配置

```
grub-mkconfig -o /boot/grub/grub.cfg
```

如果成功识别和添加 Windows 的话，应该可以看到以下类似的输出：

```
Found Windows Boot Manager on /dev/sda2@/EFI/Microsoft/Boot/bootmgfw.efi
```

到此已经大功告成了，我们可以重启验证下，如果有问题，可以留言交流。


## 相关链接

- [GRUB 记住上次启动项选择](/post/grub/remember-last-choice)
