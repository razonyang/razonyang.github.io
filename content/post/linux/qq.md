---
date: "2017-07-10T17:33:40+08:00"
comment: true
categories: ["development", "linux"]
tags: ["linux", "qq"]
title: "Linux 各发行版安装 QQ 详细教程"
---

笔者偏爱在 Linux 桌面下办公，特喜欢的是窗口置顶，让开发真的很舒服，也许这只是我个人习惯吧。
不过也有不便之处，比如工作交流需要 QQ，而腾讯并没有开发 Linux 版的（最初有，可惜已经不能用了），
不过好消息是有个 wine 版的 QQ（2012版） 可以使用，于我而言，办公交流足矣。
<!--more-->

我曾经用过不少发行版，如 Ubuntu、LinuxMint、Manjaro、OpenSUSE等，在这些发行版都安装过 QQ。
本文将介绍这几个发行版安装QQ的教程，也提供了 deb 和 rpm 包的下载。

## Ubuntu、LinuxMint

**下载**

```shell
https://pan.baidu.com/s/1kULeHIR
```

**安装**
```shell
sudo dpkg -i wine-qqintl_0.1.3-2_i386.deb
```


## OpenSUSE、Fedora

**下载**

```shell
https://pan.baidu.com/s/1i4Ro2G1
```

**安装**

```shell
sudo rpm -i wine-qqintl-0.1.3-3.i386.rpm
```

> 此 rpm 包是由 alien 命令将 deb 包转换而成，如果有问题，请下载 deb 包，然后自行转换。


## Manjaro

Manjaro 的安装比较简单，有人已经将该包上传到Arch用户软件仓库（Arch User Repository，简称 AUR），只需要使用 yaourt 命令安装即可。

```shell
yaourt wine-qqintl
```