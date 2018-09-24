---
title: "CentOS 编译安装 PHP"
date: 2018-09-24T10:02:02+08:00
comment: true
tags: ["centos", "php"]
categories: ["development"]
---

由于 pthreads 要求 PHP 的版本是线程安全的，但 CentOS 装的都是非线程安全（NTS）的版本，于是笔者打算编译安装 PHP。

<!--more-->

## 下载

```
$ wget http://172.16.138.80/2Q2W4FBD12A8C4C33197EC0860CACD1FDE48BC83D2DC_unknown_484255893A43FF32C97A3B7C1C4E99FBA33A9424_1/am1.php.net/distributions/php-7.2.10.tar.gz
```

> 下载地址：http://php.net/downloads.php


## 解压

```
$ tar -xvf ./php-7.2.10.tar.gz && cd php-7.2.10
```


## 配置

```
$ ./configure \
    --prefix=/usr/local/php/php72 \
    --enable-maintainer-zts \
    --enable-bcmath \
    --enable-exif \
    --enable-intl \
    --enable-pcntl \
    --enable-soap \
    --enable-sockets \
    --enable-fpm \
    --with-gd \
    --enable-pdo \
    --with-pdo-mysql
```

- --enable-maintainer-zts - 线程安全

> 选项含义可通过 `./configure --help` 查看，按需选择。


## 编译安装

```
$ make && make install
```

测试是否安装成功

```
$ /usr/local/php/php72/bin/php -v
PHP 7.2.10 (cli) (built: Sep 24 2018 02:53:42) ( ZTS )
Copyright (c) 1997-2018 The PHP Group
Zend Engine v3.2.0, Copyright (c) 1998-2018 Zend Technologies
```


## 依赖

### libxml2

> configure: error: libxml2 not found. Please check your libxml2 installation. 

```
$ yum install libxml2-devel
```

###  ICU

> configure: error: Unable to detect ICU prefix or no failed. Please verify ICU install prefix and make sure icu-config works.

```

```
$ yum install libicu-devel
```

### cURL

> checking for cURL 7.10.5 or greater... configure: error: cURL version 7.10.5 or later is required to compile php with cURL support

PHP 默认安装 cURL，所以去掉编译选项 `--with-curl` 即可。

### libpng

> configure: error: png.h not found.

```
$ yum install libpng-devel
```
