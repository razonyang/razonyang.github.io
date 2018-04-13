---
title: "微信内置浏览器开发常见问题及其解决方法"
date: 2018-01-14T11:21:21+08:00
comment: true
tags: ["微信", "wechat"]
categories: ["development"]
---

本文记录了笔者在微信内置浏览器中开发遇到的问题以及相应的解决方法。
<!--more-->

# 判断是否为微信浏览器

我们可以根据`User Agent`来判断当前浏览器的类型，而微信浏览器的`User Agent`中含有`MicroMessenger`字符串，那么事情就很简单了。

以下给出`JavaScript`和`PHP`判断浏览器是否微信浏览器的代码段。

## JavaScript

```js
function isWechatBrowser(){
    var userAgent = window.navigator.userAgent; 
    if (userAgent.match(/MicroMessenger/i)) { 
        return true;
    }
    
    return false;
}
```

## PHP

```php
function isWechatBrowser() { 
    if (isset($_SERVER['HTTP_USER_AGENT']) && strpos($_SERVER['HTTP_USER_AGENT'], 'MicroMessenger') !== false) { 
        return true; 
    }
    
    return false;
}
``` 

# jQuery click 事件在 iOS 中不起作用

```js
$(element).on('touchend click', function() {});
```