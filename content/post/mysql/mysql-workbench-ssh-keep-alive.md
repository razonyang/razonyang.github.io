---
title: "MySQL Workbench SSH Keep Alive"
date: 2017-12-13T15:41:13+08:00
comment: true
tags: ["mysql", "mysql-workbench"]
categories: ["development"]
---

在使用 MySQL Workbench 连接远程数据库时，时常会断开连接，需要重新连接，不胜其烦。
不过我们可以通过设置 **SSH KeepAlive** 来保持连接。
<!--more-->

# Preferences

首先设置 **SSH KeepAlive**，其值默认为0（表示禁用），单位是秒，这里笔者设置为5分钟：300。

**Edit** -> **Preferences** -> **Others** -> **Timeouts** -> **SSH KeepAlive**: 300


# Connections

修改 **SSH KeepAlive** 后，还需要修改连接的方式（**Connection Method**）为 **Standard TCP/IP over SSH**，并填写 **SSH Username**，**SSH Password** 等信息。

保存后重新连接即可。
