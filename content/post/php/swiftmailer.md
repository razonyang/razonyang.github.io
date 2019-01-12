---
title: "Swift Mailer 无法连接 SMTP 服务器"
date: 2019-01-11T13:46:48+08:00
lastmod: 2019-01-11T13:46:48+08:00
comment: true
tags: ["php", "swift mailer", "telnet"]
categories: ["development"]
---

今天发现线上的 Swift Mailer 接口无法发送邮件：**Connection could not be established with host ...**

但是本地环境运行正常，首先怀疑是配置和服务器网络导致的。
<!--more-->

## 配置

| 参数 | 值
|:---|:---
| 服务器 | **smtp.exmail.qq.com**
| 端口 | **25**
| 加密 | **tls**

## 问题排查

### 配置

经过对比，线上配置和本地一致，暂时排除配置问题。

### 网络

我们暂时认为配置是正确的，用 `telnet` 测试能否和 **smtp.exmail.qq.com** 建立连接：

```
$ telnet smtp.exmail.qq.com 25

Trying 203.205.128.15...
telnet: connect to address 203.205.128.15: Connection timed out
```

可以看到服务器连接超时，于是重新仔细查阅[官方文档](#相关链接)，发现还有一个端口是 `465`，再试一次：

```
$ telnet smtp.exmail.qq.com 465

Trying 163.177.90.125...
Connected to smtp.exmail.qq.com.
Escape character is '^]'.
```

连上了！于是乎自信满满地修改配置，重新测试下线上接口。

本以为完事了，可仍然发送失败。于是又看了下文档，发现腾讯企业邮箱 SMTP 只能用 `ssl` 加密传输，于是再次修改配置并测试，这次终于成功了。

> 腾讯企业邮箱还提供了海外的 SMTP 服务器：**hwsmtp.exmail.qq.com**。
如果你的服务器位置不在大陆，或者连不上 **smtp.exmail.qq.com**，可以尝试更换为海外服务器。


## 相关链接

- [腾讯企业邮箱 - 如何设置IMAP、POP3/SMTP及其SSL加密方式？](https://service.exmail.qq.com/cgi-bin/help?subtype=1&id=28&no=1000585)
