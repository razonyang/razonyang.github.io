---
date: 2017-06-27T12:25:48+08:00
comment: true
categories: ["development", "linux"]
tags: ["linux", "unzip"]
title: "Linux unzip 用法详解"
---

本文将讲述 `unzip` 的一些常用用法，比如`解压到制定的文件夹`、`避免中文乱码问题`等。
<!--more-->

# 解压到指定文件夹

`unzip` 如果要将压缩包解压到制定目录，则需要参数：`-d 目标文件夹名`

```shell
unzip -d specified-dir data.zip
```

# 避免中文乱码

在解压带有中文我文件名的 `zip` 压缩包，可能会出现乱码，
可以通过 `unzip` 的 `-O CHARSET` 指定字符集，以避免乱码：

```shell
unzip -O GBK data.zip
```
