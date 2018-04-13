---
title: "Hugo 转义 Shorcodes"
date: 2017-11-21T18:35:50+08:00
comment: true
tags: ["hugo"]
categories: ["development"]
---

在之前一篇博文 [Hugo Markdown 里嵌入局部模板](/post/hugo/markdown-embed-partial) 中使用到了 shortcodes,
但是需要直接输出原本的 shortcodes 呢？应该如何转义呢？其实转义只需要添加注释即可。

比如：

```
{{</* partial "for-example.html" */>}}
```

转义，左尖括号后加上`/*`，右尖括号前加上`*/`：
```
< => </*
> => */>
```
<!--more-->