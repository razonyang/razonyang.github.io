---
title: "Gentoo 安装教程 - 分区"
date: 2018-05-18T17:46:54+08:00
comment: true
tags: ["gentoo", "disk", "partition"]
categories: ["development"]
---

安装 Gentoo 之前，我们需要准备好磁盘，虽然可以使用整个硬盘，但是现实中几乎不这么做。
事不宜迟，马上开始磁盘分区。
<!--more-->

## 分区表

本教程使用 GPT + UEFI 进行分区和引导。

- [MBR](https://en.wikipedia.org/wiki/Master_boot_record)
- [GPT](https://en.wikipedia.org/wiki/GUID_Partition_Table)
- [BIOS](https://en.wikipedia.org/wiki/BIOS)
- [UEFI](https://en.wikipedia.org/wiki/Unified_Extensible_Firmware_Interface)


## 分区方案

笔者用的是比较简单的分区方案：根分区，`home` 分区，`swap` 分区，大家可以根据自己需要进行调整。

| Partition | Size      | File System | Description                                                    
|:----------|:----------|:------------|:-----------------
| BIOS      | 2 MiB     |             |                                                   
| /boot     | 256 MiB   | fat32       |                                                   
| swap      | 8 GiB     | swap        | 一般至少为内存大小                                             
| /         | 30 GiB    | ext4        | 一般 20 GiB 足够了                                             
| /home     | 剩余空间  | ext4        |                                                      


## 分区

### 磁盘标志

紧接着开始进行分区，首先用 `fdisk` 命令确定要分区的磁盘信息：

```
$ fdisk

Disk /dev/sdc: 111.8 GiB, 120034123776 bytes, 234441648 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: A4839DC8-2940-47C6-B2A7-D17F8E328411
```

可以看到我的磁盘标志是 `/dev/sdc`，后续篇章都会默认使用该磁盘标志。

### 开始分区

这里我们使用 `parted` 进行分区：

```
$ parted -a optimal /dev/sdc
GNU Parted 3.2
Using /dev/sdc
Welcome to GNU Parted! Type 'help' to view a list of commands.
(parted)
```

设置 GPT 标签：

```
(parted) mklabel gpt                                                      
Warning: The existing disk label on /dev/sdc will be destroyed and all data on this disk will be
lost. Do you want to continue?
Yes/No? y
```

设置容量单位

```
(parted) unit mib
```

BIOS 分区

```
(parted) mkpart primary 1 3
(parted) name 1 grub
(parted) set 1 bios_grub on
```

引导分区

```
(parted) mkpart primary 3 259
(parted) name 2 boot
(parted) set 2 boot on
```

`swap` 分区

```
(parted) mkpart primary 259 8451
(parted) name 3 swap
```

根分区

```
(parted) mkpart primary 8451 39171
(parted) name 4 rootfs
```

`home` 分区

```
(parted) mkpart primary 39171 -1 
(parted) name 5 home
```

查看分区信息

```
(parted) p
Model: aigo Portable SSD (scsi)
Disk /dev/sdc: 114473MiB
Sector size (logical/physical): 512B/512B
Partition Table: gpt
Disk Flags:

Number  Start     End        Size      File system  Name    Flags
 1      1.00MiB   3.00MiB    2.00MiB                grub    bios_grub
 2      3.00MiB   259MiB     256MiB                 boot    boot, esp
 3      259MiB    8451MiB    8192MiB                swap
 4      8451MiB   39171MiB   30720MiB               rootfs
 5      39171MiB  114472MiB  75301MiB               home
```

> 需要注意 `boot` 分区是否有 `esp` 的标志。

如果分区没有问题，输入 `q` 结束分区。


## 文件系统

分区后，我们还需要给分区创建相应的文件系统。

因为笔者使用的 `UEFI`，引导分区需要使用 `FAT32` 文件系统：

```
$ mkfs.fat -F 32 /dev/sdc2
mkfs.fat 4.0 (2016-05-06)
```

> 如果抱怨说 `mkfs.fat: command not found`，你只需要 `emerge --ask sys-fs/dosfstools`。

为根分区和 `home` 分区创建 `ext4` 分区：

```
$ mkfs.ext4 /dev/sdc4
$ mkfs.ext4 /dev/sdc5
```

激活 `swap` 分区：

```
$ mkswap /dev/sdc3
$ swapon /dev/sdc3
```

自此磁盘分区大功告成！接下来我们需要[安装 Gentoo 安装文件](/post/gentoo/installation-stage)。
