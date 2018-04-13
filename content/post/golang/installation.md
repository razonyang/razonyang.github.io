---
date: "2017-08-05T12:54:25+08:00"
categories: ["development"]
tags: ["golang", "linux"]
title: "Linux 搭建 Golang 开发环境"
comment: true
---

在 Linux 中安装 Golang 非常简单，只需要一条命令即可，比如在 Ubuntu 下安装 Golang：

```shell
sudo apt install golang-go
```

不过安装的版本一般都不会是最新稳定版的，所以本文将简述如何手动安装特定版本的 Golang。
<!--more-->


以当前最新的稳定版 **1.8.3** 为例：

# 下载

```shell
wget https://storage.googleapis.com/golang/go1.8.3.linux-amd64.tar.gz
```

其他版本下载地址：https://golang.org/dl


# 解压

```shell
tar -zxvf ./go1.8.3.linux-amd64.tar.gz
```


# 存放

将 GOROOT 假定为 **/usr/local/go**

```shell
sudo mv ./go /usr/local/go
```


# 环境变量

```shell
vi ~/.profile
```

添加以下内容

```shell
export GOROOT=/usr/local/go
export GOPATH="$HOME/Go"
PATH="$GOPATH/bin:$GOROOT/bin:$PATH"
```

- GOROOT - Golang 的安装路径
- GOPATH - Golang 工作空间
- PATH - 添加 **$GOPATH/bin** 和 **$GOROOT/bin**，方便使用一些 Golang 的包的命令行工具

**重载环境变量**

```shell
source ~/.profile
```