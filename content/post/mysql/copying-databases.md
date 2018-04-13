---
title: "MySQL Making a Copy of a Database"
date: 2017-11-17T16:47:01+08:00
comment: true
tags: ["mysql"]
categories: ["development", "database"]
---

MySQL拷贝一个数据库，只需要简单的几个命令。
<!--more-->

**导出数据库**

```shell
mysqldump db1 > dump.sql
```

**创建数据库**

```shell
mysqladmin create db2
```

**导入数据**

```shell
mysql db2 < dump.sql
```

> `mysqldump`不能使用`--databases`参数，因为这会使得导出的SQL会包含`USE db1`命令。

原文：[Making a Copy of a Database](https://dev.mysql.com/doc/refman/5.7/en/mysqldump-copying-database.html)