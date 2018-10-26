---
title: "CentOS 7 安装 ShadowSocks"
date: 2018-10-26T13:18:29+08:00
comment: true
tags: ["centos", "shadowsocks"]
categories: ["linux"]
---

CentOS 7 安装 ShadowSocks
<!--more-->

## 安装 Shadowsocks

```
yum install python-setuptools && easy_install pip
pip install git+https://github.com/shadowsocks/shadowsocks.git@master
```

> 如果你没有安装 Git，则先需要执行 `yum install git nss curl` 安装 Git 版本管理工具。


## 配置

安装之后，先建一个配置文件 `/etc/ssserver.json`:

```
{
    "server":"ogfw.errlogs.com",
    "server_port":8388,
    "local_address":"127.0.0.1",
    "password":"123456",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open":false
}
```

- server：服务器 IP 或域名
- server_port：端口
- password：密码
- method：加密方式

然后启动 Shadowsocks：

```
ssserver -c /etc/ssserver.json
```

接着用客户端连接测试是否成功。

> 如果失败，很可能是端口、密码、加密方式没有设置正确，又或者是防火墙配置，我们可以临时关闭防火墙 `systemctl stop firewalld` 验证是否防火墙原因。


## 服务

如果测试没有问题，接下来添加 Shadowsocks 服务 `/etc/systemd/system/ssserver.service`，让其开机自启。

```
[Unit]
Description=Shadowsocks Server
After=network.target
After=syslog.target

[Install]
WantedBy=multi-user.target

[Service]
Type=simple
PIDFile=/var/run/ssserver.pid

ExecStart=/usr/bin/ssserver -c /etc/ssserver.json

Restart=on-failure

RestartPreventExitStatus=1
```

启动：

```
systemctl start ssserver
```

查看状态：

```
systemctl status ssserver
```


如果没有问题，设置其开机自启：

```
systemctl enable ssserver
```
