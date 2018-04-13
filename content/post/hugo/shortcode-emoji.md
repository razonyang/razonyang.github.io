---
title: "Hugo 表情包 Shortcode"
date: 2017-12-08T14:25:55+08:00
comment: true
tags: ["hugo"]
categories: ["development"]
---

在前文 [Hugo Markdown 里嵌入局部模板](/post/hugo/markdown-embed-partial) 中，描述了如何使用 Shortcode 使 Markdown 得以嵌入模板。
但是对于表情包，引入局部模板不是很明智的做法，本文将讲述如何定制自己的**表情包 Shortcode**。
<!--more-->

# 需求

- 单个 Shortcode 加载各种表情包
- 可定制表情包的属性，比如 `alt`，`title`，`width`，`height` 等等

表情包实质是一张图片，要实现单个 Shortcode 加载表情包，只需要将表情包的名称传入 Shortcode，
然后 Shortcode 根据名称填充的表情包图片的 URL 即可。

而对于表情包的属性，则只是添加 img 标签的属性。

# 实现

## 表情包图片

第一步，自然是存放我们的表情包了，本文假定存放路径为：

```
static/img/emoji
```

比如：

{{< emoji name="interesting" >}}

{{< emoji name="for-example" >}}


## 创建 Emoji Shortcode

有了表情包，紧接着我们就需要创建 Shortcode 文件来加载这些表情包，如 `layouts/shortcodes/emoji.html`：

```
{{ $name := .Get "name" }}
<img
    src="/img/emoji/{{ $name }}.{{ with .Get "ext" }}{{ . }}{{ else }}jpeg{{ end }}"
    title="{{ with .Get "title" }}{{ . }}{{ else }}{{ $name }}{{ end }}"
    alt="{{ with .Get "alt" }}{{ . }}{{ else }}{{ $name }}{{ end }}"
    {{ with .Get "width" }} width="{{ . }}"{{ end }}
    {{ with .Get "height" }} height="{{ . }}"{{ end }}
/>
```

说明：

1. 首先获取传入的表情包名称 - `{{ $name := .Get "name" }}` 
2. 接着返回一个 `img` 标签


## 调用 Emoji Shortcode

最后，我们只需要调用上一步创建的 Shortcode 则大功告成。

**显示 interesting 表情包**

```
{{</* emoji name="interesting" */>}}
```

**设置属性**
```
{{</* emoji name="interesting" ext="jpeg" width="400" height="200" title="title" alt="alt" */>}}
```