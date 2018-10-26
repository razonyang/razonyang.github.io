---
title: "Gentoo 搭建 Nginx、MySQL、PHP 环境"
date: 2018-05-24T18:10:39+08:00
draft: true
comment: true
tags: ["gentoo", "nginx", "mysql", "php"]
categories: ["development"]
---

本文将介绍如何在 Gentoo 上搭建 Nginx、MySQL、PHP 环境。
<!--more-->

## PHP

### 设置版本

```
vi /etc/portage/make.conf

PHP_TARGETS="php7-1"
```

### 设置扩展

```
vi /etc/portage/package.use/php

dev-lang/php curl fpm gd intl opcache pcntl pdo session
```

### 安装

### 开启自启

## Nginx

### 配置模块

因为 PHP 需要使用到 fastcgi 模块：

```
vi /etc/portage/package.use/nginx


www-servers/nginx NGINX_MODULES_HTTP: fastcgi
```

### 安装

### 开启自启

```
emerge --ask dev-db/mysql
```

## MySQL

### 安装

```
emerge --ask dev-db/mysql
```

### 开启自启

```
rc-update add mysql default
```

### 启动

```
rc-service mysql start
```