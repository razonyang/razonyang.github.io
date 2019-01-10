---
title: "使用 Zend Guard 保护 PHP 源码"
date: 2019-01-10T17:01:09+08:00
lastmod: 2019-01-10T17:01:09+08:00
draft: true
comment: true
tags: ["php", "zend guard"]
categories: ["development"]
---

由于公司项目需求，需要将
<!--more-->


## 常见问题

### Undefined symbol: compiler_globals

> Failed loading /usr/local/php/php54/lib/php/extensions/zend_loader.so:  /usr/local/php/php54/lib/php/extensions/zend_loader.so: undefined symbol: compiler_globals

该问题可能是因为 PHP 是 ZTS(线程安全) 版，你需要安装 Non-ZTS(非线程安全) 版。
如果你打算编译安装，只需要去掉 `configure` 配置参数 `--enable-maintainer-zts` 即可（默认非线程安全）。