---
title: "Gentoo 安装教程 - Linux 内核配置"
date: 2018-09-02T09:28:28+08:00
comment: true
tags: ["gentoo", "kernel"]
categories: ["gentoo"]
---

配置 Linux 内核有两种方式：自动和手动，如果你是新手，建议用自动方式。而本文只介绍如何手动配置 Linux 内核，这是因为手动配置生成内核所需的时间更短，大小更小。虽然对于首次手动配置的童鞋可能比较困难，但是熟能生巧。
<!--more-->

当然，如果你想自动配置，可以查阅官方文档 - https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Kernel/zh-cn

废话少说，我们马上进入正题。

## 安装源码

```
$ emerge --ask sys-kernel/gentoo-sources
```

## 安装 PCI 工具

```
$ emerge --ask sys-apps/pciutils
```

安装之后，查看当前 LIVE CD 环境使用到的内核模块。

```
$ lspci -vv
07:00.0 Network controller: Intel Corporation Wireless 7265 (rev 48)
        Subsystem: Intel Corporation Dual Band Wireless-AC 7265
        Control: I/O- Mem+ BusMaster+ SpecCycle- MemWINV- VGASnoop- ParErr- Stepping- SERR- FastB2B- DisINTx+
        ...
        Kernel driver in use: iwlwifi

08:00.0 Ethernet controller: Realtek Semiconductor Co., Ltd. RTL8111/8168/8411 PCI Express Gigabit Ethernet Controller (rev 0c)
        Subsystem: Realtek Semiconductor Co., Ltd. RTL8111/8168/8411 PCI Express Gigabit Ethernet Controller
        Control: I/O+ Mem+ BusMaster+ SpecCycle- MemWINV- VGASnoop- ParErr- Stepping- SERR- FastB2B- DisINTx+
        ...
        Kernel driver in use: r8169
```

这里笔者只截取无线和有线网卡部分，我们主要看 `Kernel driver in use: *`，比如 `iwlwifi` 和 `r8169`，后续配置要使用到。


## 配置内核

```
$ cd /usr/src/linux
$ make menuconfig
```

### 操作说明

进入配置界面后，可以看到一个带有层级的菜单列表，这里笔者简单介绍下如何操作：

- 回车 - 进入子菜单
- Y - 包含选项
- M - 作为模块
- N - 排除选项
- ? - 帮助信息
- / - 搜索，输入配置名称，然后回车搜索，然后输入结果序号即可跳转到相应的配置位置

### 配置模块

由于菜单选项繁多，一层一层寻找十分烦闷，这里笔者只列出模块名称，按 `/` 搜索即可找到需要的选项位置。

- 必要选项 - `CONFIG_DEVTMPFS`，`CONFIG_DEVTMPFS_MOUNT`，`CONFIG_BLK_DEV_SD`
- 文件系统 - `CONFIG_EXT4_FS`
- EFI - `CONFIG_PARTITION_ADVANCED`，`CONFIG_EFI_PARTITION`，`CONFIG_EFI`, `CONFIG_EFI_STUB`, `CONFIG_EFI_MIXED`, `CONFIG_EFI_VARS`
- 无线网卡 - `iwlwifi`，这里最好编译成模块，也就是 `M`，因为笔者之前安装编译到内核发现不起作用。
- 有线网卡 - `r8169`

> 这里的选项配置几乎和你机器相关，比如无线和有线网卡的模块名，请注意查看 `lspci` 提供的信息。如果你对内核配置还是没有头绪，可以先查阅下文章底部的扩展阅读。

### 编译安装

```
$ make && make modules_install
$ make install
```

中场休息～


## 生成 initramfs

`initramfs` 是一个基于内存的初始化文件系统，此步骤可选，一般一些重要的文件系统位置（如 /usr，/var）在单独的分区时，则需要生成 initramfs，否则可能无法正常开机。

虽然是可选，但是笔者还是建议安装。

```
$ emerge --ask sys-kernel/genkernel
$ genkernel --install initramfs
```

## 安装固件

```
$ emerge --ask sys-kernel/linux-firmware
```

自此，内核配置完成，接下来需要[配置系统](/post/gentoo/installation-system)了。

## 扩展阅读

- [Gentoo 配置 Linux 内核](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Kernel/zh-cn)
- [Gentoo 内核配置指南](https://wiki.gentoo.org/wiki/Kernel/Gentoo_Kernel_Configuration_Guide/zh-cn)
