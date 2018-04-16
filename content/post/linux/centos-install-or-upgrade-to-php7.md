---
title: "CentOS 安装或升级到 PHP7"
date: 2018-04-16T13:10:26+08:00
comment: true
tags: ["centos","php","php7"]
categories: ["development"]
---

CentOS 官方仓库的 PHP 版本是 5.4，但此版本已经不再被官方支持，考虑到安全问题和更好的性能，我们有必要安装或升级更高版本的 PHP，比如最新的 7.2。
<!--more-->

那么我们进入正文，首先我们需要安装 EPEL 和 Remi，至于这两个是什么，有兴趣的童鞋可以在相关链接进行查阅。

## 仓库

### 安装 EPEL

```
yum install epel-release
```

### 安装 Remi

```
yum install https://rpms.remirepo.net/enterprise/remi-release-7.rpm
```

### 启用 PHP 仓库

```
yum-config-manager --enable remi-php72
```

你也可以选择其他版本，如：`remi-php56`，`remi-php70`，`remi-php71`

## 安装或升级

### 安装 PHP

```
yum install php
```

### 升级 PHP

```
yum update
```

## 相关链接

- [Extra Packages for Enterprise Linux](https://fedoraproject.org/wiki/EPEL)
- [What is Remi repository](https://blog.remirepo.net/pages/English-FAQ#goal)
- [Install PHP 7 on CentOS, RHEL or Fedora](https://blog.remirepo.net/post/2016/02/14/Install-PHP-7-on-CentOS-RHEL-Fedora)