---
title: "Gentoo 安装教程 - 安装文件"
date: 2018-09-01T22:02:27+08:00
comment: true
tags: ["gentoo"]
categories: ["gentoo"]
---

我们磁盘分区后，在安装 Gentoo 基本系统之前，需要安装 Gentoo 的安装文件。
<!--more-->

## 时间日期

在安装之前，需要确保日期和时间设置正确，否则会出现奇怪的结果。

```
$ date
Sat Sep  1 22:07:43 CST 2018
```

如果你的时间不正确，可以通过自动或手动方式修改时间：

- 自动 - `ntpd -q -g`
- 手动 - `date MMDDhhmmYYYY`（月,日,小时,分钟 和 年），比如 `date 090122072018`


## Stage 包

### 挂载分区

接下来安装 Stage 包，因为 Stage 包最终是需要解压到我们的分区里面的，所以我们需要先挂载磁盘分区：

```
$ mkdir /mnt/gentoo
$ mount /dev/sdc4 /mnt/gentoo
$ cd /mnt/gentoo && mkdir boot home
$ mount /dev/sdc2 /mnt/gentoo/boot
$ mount /dev/sdc5 /mnt/gentoo/home
```

### 下载 Stage 包

挂载分区后，我们先下载 Stage 包，下载地址可在 https://www.gentoo.org/downloads/ 找到。

> 建议用国内的网易镜像，下载速度快。

```
$ wget http://mirrors.163.com/gentoo/releases/amd64/autobuilds/20180830T214502Z/stage3-amd64-20180830T214502Z.tar.xz
```

> 注意要先切换到 `/mnt/gentoo` 目录，因为后续命令都是默认其为当前的目录。


### 解压 Stage 包

```
$ tar xpfv stage3-amd64-20180830T214502Z.tar.xz --xattrs-include='*.*' --numeric-owner 
```

> 注意！解压之前，先确保当前解压的目录为 `/mnt/gentoo`，可以通过 `pwd` 检查当前所处目录。当前你也可以用 `tar` 指定解压的目标目录，不过为了简单和避免不必要的问题，最好还是先切换到 `/mnt/gentoo` 目录，再进行安装操作！

参数说明：

- `x` - extract，意为解压
- `p` - **p**reserve permissions, 保留权限
- `f` - 表明我们要解压的是一个文件
- `v` - verbose，可选
- `--xattrs-include` - 保留文档的扩展属性 
- `--numeric-owner` - 保留文件的 user 和 group IDs

## 修改编译选项

```
$ nano -w /mnt/gentoo/etc/portage/make.conf

CFLAGS="-march=native -O2 -pipe"
# Use the same settings for both variables
CXXFLAGS="${CFLAGS}"

# 一般设置为 CPU 核心数 +1，可以通过命令 `nproc --all` 查看自己电脑的 CPU 核心数
MAKEOPTS="-j9"
```

> 题外话，Gentoo 的 LIVE CD 没有自带 `vi`，而是 `nano`。


到此万事具备，我们现在可以[安装 Gentoo 基本系统](/post/gentoo/installation-base)。
