---
title: "Git 配置"
date: 2018-09-24T12:12:46+08:00
lastmod: 2018-09-24T12:12:46+08:00
comment: true
tags: ["git"]
categories: ["development", "git"]
---

Git 常用配置命令。
<!--more-->

## 帐号邮箱

```
$ git config --global user.name "razonyang"
$ git config --global user.email "razonyang@gmail.com"
```


## 编辑器

我们可以修改 Git 的编辑器为 vim：

```
$ git config --global core.editor "vim"
```


## 缓存密码

```
$ git config --global credential.helper 'cache --timeout=3600'
```

- timeout - 有效期，例子中为一小时。
