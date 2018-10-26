---
title: "Gentoo 安装 Nginx"
date: 2018-07-30T00:58:03+08:00
comment: true
tags: ["gentoo", "nginx", "lnmp"]
categories: ["development"]
---

无论在本地开发，还是线上服务器，Nginx 都是笔者首选的服务器软件，本文将简述如何在 Gentoo 安装 Nginx。
<!--more-->

## USE flags

在安装前，我们需要配置一些必要的模块，如 fastcgi：

```
$ vi /etc/portage/package.use/nginx

www-servers/nginx http http-cache http2 NGINX_MODULES_HTTP: fastcgi
```

这里只列出了笔者需要的模块，更多的标记和模块可以使用 `emerge --ask --pretend nginx` 查看。 


## 安装

```
$ emerge --ask www-servers/nginx
```


## 创建 Web 目录和文件

```
$ mkdir /var/www/localhost/htdocs
$ echo 'Hello, world!' > /var/www/localhost/htdocs/index.html
```


## 启动 Nginx

```
$ /etc/init.d/nginx start
```


## 验证

如果安装成功，应该可以得到以下类似结果：

```
$ curl http://localhost

Hello, world!
```


## 开机启动

```
$ rc-update add nginx default
```


## PHP 配置

由于笔者要搭建 lnmp 环境，自然少不了 PHP 的配置：

```
$ vi /etc/nginx/nginx.conf

server {
	# ...

	location ~* \.php$ {
		fastcgi_pass    127.0.0.1:9000;
		include         fastcgi_params;
		fastcgi_param   SCRIPT_FILENAME    $document_root$fastcgi_script_name;
		fastcgi_param   SCRIPT_NAME        $fastcgi_script_name;
	}

	# ...
}
```

验证：

```
$ echo '<?php echo "Hello foo!";' > /var/www/localhost/htdocs/index.php
$ rc-service nginx restart
$ curl http://localhost/index.php

Hello foo!
```


## 相关文章

- [Gentoo 安装 PHP](/post/gentoo/php)
- [Gentoo 安装 MySQL](/post/gentoo/mysql)
