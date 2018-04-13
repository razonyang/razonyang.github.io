---
title: "开启 Redis 远程访问"
date: 2018-04-13T14:08:03+08:00
comment: true
tags: ["redis"]
categories: ["development"]
---

本文阐述如何开启 Redis 的远程访问，以及相关问题的原因及解决方法。
<!--more-->

## 环境

- host - redis.errlogs.com
- port - 6379
- redis
    - version - 3.2.10


## Redis 配置

默认安装的 Redis 服务器，是不允许远程连接的，因此需要修改相关配置：


### bind

注释 bind 指令，以允许远程连接。

```
# bind 127.0.0.1
```

> Note: bind 并不是类似 IP 白名单的东西，别问我为啥知道...

### requirepass

由于注释了 bind 指令，这将你的 Redis 服务器暴露给所有网络上的主机，安全起见，我们需要设置下密码：

```
requirepass yourSecretPasswd
```


## 连接远程 Redis 服务器

本文使用命令行 `redis-cli` 进行连接：

```
redis-cli -h {host} -p {port}
```

- `-h` - 指定远程服务器
- `-p` - 指定端口，默认为6379，可省略

如：

```
redis-cli -h redis.errlogs.com -p 6379
```


## 问题汇总


### Connection timed out - 连接超时

> Could not connect to Redis at redis.errlogs.com:6379: Connection timed out

如果连接时，显示上述错误，罪魁祸首很可能是`防火墙`，至少有两条线索可以断定：

1. 关闭防火墙，简单粗暴。

2. 开启 Redis 端口，如`6379`。至于如何开启防火墙端口，已超出本文范畴，请读者自行解决...

然后重新连接，一般情况下，连接成功，如有意外，纯属意外...


### Connection refused - 拒绝连接

> Could not connect to Redis at redis.errlogs.com:6379: Connection refused

若出现上述错误，一般是 Redis 配置问题，请仔细检查 Redis 配置，然后重启 Redis 服务，重新连接。


### DENIED

> (error) DENIED Redis is running in protected mode because protected mode is enabled, no bind address was specified, no authentication password is requested to clients.
 In this mode connections are only accepted from the loopback interface.
 If you want to connect from external computers to Redis you may adopt one of the following solutions:
 1\) Just disable protected mode sending the command 'CONFIG SET protected-mode no' from the loopback interface by connecting to Redis from the same host the server is running,
      however MAKE SURE Redis is not publicly accessible from internet if you do so. Use CONFIG REWRITE to make this change permanent.
 2\) Alternatively you can just disable the protected mode by editing the Redis configuration file, and setting the protected mode option to 'no', and then restarting the server.
 3\) If you started the server manually just for testing, restart it with the '--protected-mode no' option.
 4\) Setup a bind address or an authentication password.
 NOTE: You only need to do one of the above things in order for the server to start accepting connections from the outside.

简而言之，是指 Redis 开启了 protected mode，但既没有 bind 任何 IP，也没有设置密码。我们可以通过以下方式解决此问题：

1. 在 Redis 所在服务器上执行 `CONFIG SET protected-mode no`，此命令在 Redis 重启后失效，不过可以通过执行命令 `CONFIG REWRITE` 使其永久生效。
2. 修改配置文件中的 `protected-mode yes` => `protected-mode no`，然后重启即可。
3. 也可以通过命令行启动 Redis - `redis-server --protected-mode no`。
4. `bind` IP 或者设置密码。

安全起见，个人只推荐第四种方法 - 开启密码：

```
requirepass yourSecretPasswd
```

### 其他错误

如果还是连接不上，请仔细检查 Redis 服务是否启动，修改配置后有无重启 Redis 服务等原因。