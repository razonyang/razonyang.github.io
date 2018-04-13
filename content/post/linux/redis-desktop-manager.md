---
date: "2017-08-08T10:50:44+08:00"
comment: true
categories: ["development", "linux"]
tags: ["linux", "redis"]
title: "Linux 安裝 Redis Desktop Manager"
---

为了更好的操作 Redis，最近找到了一款名为 [Redis Desktop Manager](https://redisdesktop.com/) Redis 客戶端，
但按照官方的安装教程进行编译，出现了种种问题，所以记录下来，以便帮助他人。
<!--more-->

# 问题

git clone --recursive 的时候，有几个模块无法下载，所以就放弃编译方式安装

- https://chromium.googlesource.com/breakpad/breakpad - 被墙，Github 上有镜像
- https://github.com/uglide/qgamp - NOT FOUND，应该是被设为私有仓库了
- https://github.com/uglide/php-unserialize-js - NOT FOUND，应该是被设为私有仓库了

**后续**

现在的主分支已经修改了 **.gitmodules**，有兴趣可以尝试用最新的代码进行编译安装。


# 下载

Github 上有已经打包好的安装包 https://github.com/uglide/RedisDesktopManager/releases。

不过在我这边都打不开，所以提供了个 [redis-desktop-manager_0.9.0.17_amd64.deb](https://pan.baidu.com/s/1o8jwL0A) 的安装包下载地址，其他版本的自行搜索即可。