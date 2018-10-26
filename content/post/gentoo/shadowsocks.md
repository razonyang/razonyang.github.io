---
title: "Gentoo 安装 Shadowsocks 进行科学上网"
date: 2018-05-24T19:01:52+08:00
comment: true
tags: ["gentoo", "shadowsocks"]
categories: ["development"]
---

相信不少童鞋都是自己搭 Shadowsocks 进行科学上网的，正好笔者正在将服务器系统由 CentOS 转成 Gentoo，顺便分享下如何在 Gentoo 下安装 Shadowsocks 进行科学上网。
<!--more-->

## 安装

先安装 Shadowsocks：

```
emerge --ask net-proxy/shadowsocks-libev
```

## 配置

紧接着修改配置文件 `/etc/shadowsocks-libev/shadowsocks.json`：

```
{
    "server_host": "ogfw.errlogs.com",
    "server_port": "8838",
    "local_port": "1080",
    "password": "password",
    "timeout": 600,
    "method": "aes-256-cfb"
}
```

- server_host - 服务器 host 或者 IP
- server_port - 服务器端口
- local_port - 本地服务器端口
- method - 加密方式，可以通过 `ss-server --help` 查看所支持的加密方式
- password - 密码


## 启动

配置完成后，启动 Shadowsocks，并用客户端连接测试是否成功。

```
rc-service shadowsocks.server start
```

> 其进程 ID 存放在 `/run/shadowsocks.pid`

如果客户端连接失败，一般是配置问题，请仔细检查端口、密码、加密方式等！


## 停止

```
rc-service shadowsocks.server stop
```

## 开机自启

```
rc-update add shadowsocks.server default
```