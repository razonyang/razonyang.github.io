---
title: "PHP 7 错误处理"
date: 2018-12-06T16:37:35+08:00
lastmod: 2018-12-06T16:37:35+08:00
comment: true
tags: ["php", "error", "exception", "throwable"]
categories: ["development", "php"]
---

相较于 PHP 5, PHP 7 改变了大多数错误的报告方式，其将错误作为 **Error** 异常抛出。

因为 **Error** 并非继承自 **Exception**，如果你的运行环境混杂着 PHP 5 和 PHP 7 时，需要两个 `catch` 块。

有意思的是 **Error** 和 **Exception** 均实现了 **Throwable** 接口，但是如果仅用以下异常处理代码在 PHP 5 下并不起作用：

```php
try {
    // ...
} catch (\Throwable $t) {
    echo $t->getMessage() . PHP_EOL;
}
```

## 兼容 PHP 5

所以如果要兼容 PHP 5，我们还需要添加 **Exception** 的 catch 块：
<!--more-->

```php
try {
    // ...
} catch (\Throwable $t) {
    echo $t->getMessage() . PHP_EOL;
} catch (\Exception $e) {
    echo $e->getMessage() . PHP_EOL;
}
```
