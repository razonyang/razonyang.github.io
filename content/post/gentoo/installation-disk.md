---
title: "Installation Disk"
date: 2018-05-18T17:46:54+08:00
draft: true
comment: true
tags: ["gentoo", "disk", "partition"]
categories: ["development"]
---

<!--more-->

| Partition | Size    | Description                                                    |
|:----------|:--------|:---------------------------------------------------------------|
| BIOS      | 2 MiB   |                                                                |
| /boot     | 128 MiB |                                                                |
| swap      | 8 GiB   |                                                                |
| /         | 20 GiB  |                                                                |
| /var      | 20 GiB  |                                                                |
| /usr      | 10 GiB  |                                                                |
| /etc      | 1 GiB   |                                                                |
| /home     | 剩余空间  |                                                                |


```
parted -a optimal /dev/sdb
```

```
(parted) mklabel gpt

(parted) unit mib

(parted) mkpart primary 1 3
(parted) name 1 grub
(parted) set 1 bios_grub on

(parted) mkpart primary 3 131
(parted) name 2 boot
(parted) set 2 boot on

(parted) mkpart primary 131 8323
(parted) name 3 swap

(parted) mkpart primary 8323 28803
(parted) name 4 rootfs

(parted) mkpart primary 28803 29827
(parted) name 5 etc

(parted) mkpart primary 29827 40067
(parted) name 6 usr

(parted) mkpart primary 40067 60547
(parted) name 7 var

(parted) mkpart primary 60547 -1
(parted) name 8 home

(parted) print
(parted) q
```

## 创建文件系统

```
mkfs.vfat -F 32 /dev/sdb2

mkfs.ext4 /dev/sdb4
mkfs.ext4 /dev/sdb5
mkfs.ext4 /dev/sdb6
mkfs.ext4 /dev/sdb7
mkfs.ext4 /dev/sdb8
```

## 挂载分区

```
mount /dev/sda4 /mnt/gentoo

mkdir /mnt/gentoo/etc
mount /dev/sda5 /mnt/gentoo/etc

mkdir /mnt/gentoo/usr
mount /dev/sda6 /mnt/gentoo/usr

mkdir /mnt/gentoo/var
mount /dev/sda7 /mnt/gentoo/var

mkdir /mnt/gentoo/home
mount /dev/sda8 /mnt/gentoo/home
```