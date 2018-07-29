---
title: "Gentoo 安装多个版本 PHP"
date: 2018-07-29T18:35:54+08:00
comment: true
tags: ["gentoo", "php", "php-fpm", "lnmp"]
categories: ["development"]
---

本文将简述如何在 Gentoo 中安装 PHP。
<!--more-->

## 版本

要安装扩展，我们首先需要指定 PHP 的版本，这里笔者由于是 PHP ：

```
$ vi /etc/portage/make.conf

PHP_TARGETS="php5-6 php7-0 php7-1 php7-2"
```


## USE flags

配置版本后，我们设置 USE 标记，以注明要安装的扩展，请按照各自需要设置：

```
$ vi /etc/portage/package.use/php

dev-lang/php bcmath cli exif fileinfo filter fpm gd hash intl json opcache pcntl pdo simplexml soap threads xml
```

> 如果你打算使用 Apache，可以添加 `apache2`。


## 安装

配置完成后，我们安装 PHP 即可：

```
emerge --ask --update --changed-use --deep @world

emerge --ask =dev-lang/php-5.6.37 =dev-lang/php-7.0.31 =dev-lang/php-7.1.20 =dev-lang/php-7.2.8
```

> 具体版本号可以在 https://packages.gentoo.org/packages/dev-lang/php 找到。

验证安装：

```
$ php -v
PHP 7.1.18 (cli) (built: Jul 29 2018 18:53:48) ( ZTS )
Copyright (c) 1997-2018 The PHP Group
Zend Engine v3.1.0, Copyright (c) 1998-2018 Zend Technologies
    with Zend OPcache v7.1.18, Copyright (c) 1999-2018, by Zend Technologies
```

这说明默认使用的是 PHP 7.1。

我们当然也可以切换改所使用的 PHP 版本。


## 切换版本

本例以切换 cli 版本为例，如果需要切换其他的 SAPI 版本，只需要修改 cli 即可，比如 `fpm`，`cgi`，`apache2`等。

列出可选的版本：

```
$ eselect php list cli
  [1]   php5.6
  [2]   php7.0
  [3]   php7.1 *
  [4]   php7.2
```

这里可以看到默认使用的 PHP CLI 版本是 PHP 7.1。假设我们需要切换到 7.2，可以使用以下命令：

```
$ eselect php set cli 4
```

验证：

```
$php -v
PHP 7.2.8 (cli) (built: Jul 29 2018 19:26:46) ( ZTS )
Copyright (c) 1997-2018 The PHP Group
Zend Engine v3.2.0, Copyright (c) 1998-2018 Zend Technologies
    with Zend OPcache v7.2.8, Copyright (c) 1999-2018, by Zend Technologies
```


## PHP-FPM 开机启动

```
$ rc-update add php-fpm default
```


## 相关文章

- [Gentoo 安装 Nginx](/post/gentoo/nginx)
- [Gentoo 安装 MySQL](/post/gentoo/mysql)
