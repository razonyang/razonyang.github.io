---
date: "2017-07-19T09:43:18+08:00"
comment: true
categories: ["development"]
tags: ["git"]
title: "Git 缓存用户名和密码"
---

在使用 Git 的时候，经常 git pull, git push 等操作时需要输入用户名和密码，
我们可以将用户名和密码缓存起来，提高工作效率。
<!--more-->

```shell
git config --global credential.helper 'cache --timeout 3600'
```

> 其中的缓存时间单位是**秒**。