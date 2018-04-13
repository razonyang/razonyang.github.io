---
title: "JSON WEB TOKEN 详解"
date: 2017-12-15T11:59:23+08:00
draft: true
comment: true
tags: ["jwt"]
categories: ["development"]
---
ss
<!--more-->

# 简介

# 用途

# 结构

JSON Web Tokens 包含三部分，每个部分之间用 `.` 分隔开：

- Header
- Payload
- Signature

比如：

```
xxxxx.yyyyy.zzzzz
```

每个部分都是

接下来讲逐一介绍各个部分。

## Header

Header 一般包含两个部分： token 的类型（`typ`）和所使用的散列算法（`alg`）。

比如：

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

Header 是一个 JSON 字符串，需要将其进行 Base64Url 编码之后，作为 JWT 的第一个组成部分。

## Payload

载荷（Payload）是 JWT 的第二个组成部分，它包含了一系列的 claims。
Claims 分为三种类型： registered, public, and private claims，其包含实体信息（entity，比如用户信息）和额外数据。

### Registered claims

Registered claims 是预定义的一组 Claims，推荐使用但并不强制。

| Claim | Optional | Introduce                                                                                              |
|:------|:---------|:-------------------------------------------------------------------------------------------------------|
| iss   | Yes      | Issuer，发行者，该值应为一个区分大小写的字符串或者 URI                                                        |
| sub   | Yes      | Subject，主题，该值应为一个区分大小写的字符串或者 URI，且在上下文或者全局唯一                                     |
| aud   | Yes      | Audience，使用者，该值应为数组，每项都是区分大小写的字符串或者 URI。特别地，在只有一个 Audience 的情况下，该值为字符串 |
| exp   | Yes      | Expiration Time，过期时间，数值                                                                          |
| nbf   | Yes      | Not Before，时间数值，按字面理解，也就是说在 nbf 时或之后，JWT 才能够被处理                                     |
| iat   | Yes      | Issued At，发行时间，数值                                                                                |
| jti   | Yes      | JWT ID，JWT 的唯一标识                                                                                   |

> 因为 JWT 应该是紧凑的，所以 registered claims 的名称都是三个字符。

### Public claims

Public claims 是公用的，一些 public claims 可以在 [IANA JSON Web Token Registry](https://www.iana.org/assignments/jwt/jwt.xhtml) 找到，
比如 email, name, nickname 等等。

### Private claims

Private claims 是自定义的 claims，其不能和 registered claims 和 public claims 冲突。

Payload 例子：

```json
{
  "issuer": "errlogs.com",
  "name": "Razon Yang",
  "admin": true
}
```

与 Header 相同，Payload 也需要经过 Base64Url 编码之后，作为 JWT 的第二个部分。

## Signature

Signature 是为了验证 JWT 的 Header 和 Payload 是否被修改。签名的生成方法如下：

```
HMACSHA256(
  base64UrlEncode(header) + "." + base64UrlEncode(payload),
  secret
)
```

