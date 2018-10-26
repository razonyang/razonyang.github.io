---
title: "Gentoo 安装 MySQL"
date: 2018-07-30T00:37:15+08:00
comment: true
tags: ["gentoo", "mysql", "lnmp"]
categories: ["development"]
---

笔者日常工作开发基本离不开 MySQL，所以记录一下如何在 Gentoo 下安装 MySQL。
<!--more-->

## 安装

```
$ emerge --ask dev-db/mysql
```

但是默认安装的是 MySQL 5.6，而笔者想安装的是 5.7，所以需要改成：

```
$ emerge --ask =dev-db/mysql-5.7.22
```

> 5.7.22 是 MySQL 的具体版本号，如果你想安装其他版本，可以从这里获取相关的版本信息：https://packages.gentoo.org/packages/dev-db/mysql


## 开机启动

```
rc-update add mysql default
```


## 初始化

首次安装之后，我们需要初始化 MySQL，比如密码。

```
$ emerge --config dev-db/mysql
```


## 启动 MySQL

初始化之后，即可启动 MySQL：

```
rc-service mysql start
```

至此大功告成。

## 相关文章

- [Gentoo 安装 PHP](/post/gentoo/php)
- [Gentoo 安装 Nginx](/post/gentoo/nginx)
