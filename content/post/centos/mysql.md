---
title: "CentOS 安装 MySQL"
date: 2018-12-29T18:14:07+08:00
lastmod: 2018-12-29T18:14:07+08:00
comment: true
tags: ["centos", "mysql"]
categories: ["development"]
---

CentOS 安装 MySQL
<!--more-->

## 安装仓库

由于 CentOS 默认的数据库是 MariaDB，我们需要手动下载和安装 MySQL 的仓库。

下载地址：https://dev.mysql.com/downloads/repo/yum/ ，比如：

- 8.0：https://repo.mysql.com//mysql80-community-release-el7-1.noarch.rpm
- 5.7：https://repo.mysql.com//mysql57-community-release-el7-11.noarch.rpm

```
# wget https://repo.mysql.com//mysql57-community-release-el7-11.noarch.rpm
# yum install mysql57-community-release-el7-11.noarch.rpm
```


## 安装

```
# yum install mysql-server
```


## 初始化

首先需要启动服务：

```
# systemctl start mysqld
```

查看临时密码：

```
# cat /var/log/mysqld.log | grep "temporary password"
2018-12-29T10:20:14.317126Z 1 [Note] A temporary password is generated for root@localhost: wl:>g!cvK3kd
```

初始化：

```
# mysql_secure_installation
```

> 初始化会提示输入 root 密码，也就是上一步获得的 `wl:>g!cvK3kd`


## 测试

```
# mysql -u root -p

mysql> select version();
+-----------+
| version() |
+-----------+
| 5.7.24    |
+-----------+
1 row in set (0.00 sec)
```


## 开机启动

```
# systemctl enable mysqld
```