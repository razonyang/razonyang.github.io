---
title: "Hugo Markdown 里嵌入局部模板"
date: 2017-11-21T17:44:17+08:00
comment: true
tags: ["hugo"]
categories: ["development"]
---

笔者在编写博文时，喜欢使用一些表情包，比如：

{{< emoji name="for-example" >}}

但是对于相同的表情包，如果每篇博文都复制相同的代码段，
后续修改时就苦不堪言了，于是乎笔者开始寻求可以复用的方法。

> 由于博客是使用 Hugo 搭建的，所以本文只适用 Hugo。
<!--more-->

# 尝试

当时脑海浮现的第一个想法：是否可以在 Markdown 里面嵌入局部模板呢？

于是乎，创建了一个局部模板 `for-example.html`：

```html
<img src="/img/emoji/for-example.jpeg" alt="for example" title="for example" width="150!important">
```

接着，使用局部模板:

```
{{ partial "for-example.html" }}
```

但是事情发展并没有那么顺利，于是查看官方文档和搜索相关资料，最后皇天不负有心人找到了解决方案。

# 解决方案

> Shortcodes are a means to consolidate templating into small, reusable snippets that you can embed directly inside of your content.
 In this sense, you can think of shortcodes as the intermediary between page and list templates and basic content files.

也就是说，shortcodes 可以使得内容里嵌入模板。

首先，在 `layouts/shortcodes` 目录下创建一个 `partial.html`，用于嵌入局部模板：

```
{{ partial (.Get 0) }}
```

> 其中 `(.Get 0)` 获取 shortcodes 的第一个参数，此处用于获取局部模板的名称。

接着，需要创建需要复用的局部模板，因为我一开始已经创建了局部模板（`for-example.html`），跳过这一步。

最后，我们只需要在 Markdown 里，使用 shortcodes：

```
{{</* partial "for-example.html" */>}}
```

> 这里的 `partial` 是指 `layouts/shortcodes/partial.html`，`for-example.html` 是模板的名称。

至此大功告成，以后修改更新都非常方便！

文档：[Hugo Shortcodes](https://gohugo.io/templates/shortcode-templates/)