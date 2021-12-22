---
layout: wiki
title: Markdown 常见用法
tags: [Markdown]
description: Markdown 常用语法示例。
keywords: Markdown
mathjax: true
---

包含部分 `Kramdown` 用法，主要方便之后写博客参考。

---

**目录**

* TOC
{:toc}

```
* TOC
{:toc}
```

## 这里是 h2

### 这里是 h3

#### 这里是 h4

##### 这里是 h5

###### 这里是 h6

```
## 这里是 h2
### 这里是 h3
#### 这里是 h4
##### 这里是 h5
###### 这里是 h6
```

## 段落

段落一段落一段落一段落一段落一段落一段落一段落一段落一段落一段落一段落一段落一段落一段落一段落一段落一段落一段落一段落一段落一段落一段落一段落一段落一段落一段落一段落一段落一段落一段落一段落一段落一段落一段落一段落一段落一段落

段落二段落二段落二段落二段落二段落二段落二段落二段落二段落二段落二段落二段落二段落二段落二段落二段落二段落二段落二段落二段落二段落二段落二段落二段落二段落二段落二段落二段落二段落二段落二段落二段落二段落二段落二段落二段落二段落

## 超链接

[TMaize Blog](http://blog.tmaize.net)

```
[TMaize Blog](http://blog.tmaize.net)
```

## 引用

> 这里是引用

```
> 这里是引用
```

## 常见字体样式

_斜体_

**粗体**

~~删除线~~

```
_斜体_
**粗体**
~~删除线~~
```

## 列表

- 无序列表 1-1

  缩进 2 空格

  - 缩进 2 空格
  - 缩进 2 空格

- 无序列表 1-2
- 无序列表 1-3

* 无序列表项 1

* 无序列表项 2

* 无序列表项 3

```
* 无序列表项 1

* 无序列表项 2

* 无序列表项 3
```

1. 有序列表 1-1

   缩进 3 空格

   1. 缩进 3 空格
   2. 缩进 3 空格

2. 有序列表 1-2
3. 有序列表 1-3

```
- 无序列表 1-1

  缩进 2 空格

  - 缩进 2 空格
  - 缩进 2 空格

- 无序列表 1-2
- 无序列表 1-3

1. 有序列表 1-1

   缩进 3 空格

   1. 缩进 3 空格
   2. 缩进 3 空格

2. 有序列表 1-2
3. 有序列表 1-3
```

- [x] 任务列表 1
- [ ] 任务列表 2

```
- [x] 任务列表 1
- [ ] 任务列表 2
```

## 分割线

---

```
---
```

## 图片

![QANet 模型结构](https://cloud.chenkun.pro/img/2018-06-05-01.png){:height="50%" width="50%"}_图 1 QANet 模型结构_

```
![QANet 模型结构](https://cloud.chenkun.pro/img/2018-06-05-01.png){:height="50%" width="50%"}_图 1 QANet 模型结构_

// {:height="50%" width="50%"}可用于图片放缩（kramdown自带）
// 阿里云oss自带放缩会同时压缩画质
```

## 代码行

这是一段文字 `rm -rf /*` 这是一段文字

```
这是一段文字`rm -rf /*`这是一段文字
```

## 代码块

```javascript
blog.encodeHtml = function(html) {
  var o = document.createElement('div')
  o.innerText = html
  var temp = o.innerHTML
  o = null
  return temp
}
```

```javascript
blog.encodeHtml = function(html) {
  var o = document.createElement('div')
  o.innerText = html
  var temp = o.innerHTML
  o = null
  return temp
}
```

## 表格

| HEADER1 | HEADER2 | HEADER3 | HEADER4 |
| ------- | :------ | :-----: | ------: |
| content | content | content | content |

```
| HEADER1 | HEADER2 | HEADER3 | HEADER4 |
| ------- | :------ | :-----: | ------: |
| content | content | content | content |
```

1. :----- 表示左对齐
2. :----: 表示中对齐
3. -----: 表示右对齐

## 数学公式

这是一行话 $\( \int_0^\infty \frac{x^3}{e^x-1}\,dx = \frac{\pi^4}{15} \)$ 这是一行话

$$\text{If } A_1, A_2, \ldots \in \mathcal R \Rightarrow \bigcup_{n=1}^\infty A_n \in \mathcal R$$

$$
R_{\mu \nu} - {1 \over 2}g_{\mu \nu}\,R + g_{\mu \nu} \Lambda
= {8 \pi G \over c^4} T_{\mu \nu}
$$

## 锚点

```
* [引用](#引用)
```

* [引用](#引用)

## Emoji

:camel:
:blush:
:smile:

## Inline gist

<script src="https://gist.github.com/const7/51e558dcdb4a5971f380890378c944f0.js"></script>

```javascript
<script src="https://gist.github.com/const7/51e558dcdb4a5971f380890378c944f0.js"></script>
```

## Footnotes

This is a text with footnote[^1].

```md
This is a text with footnote[^1].
```

## 插入 html

<div id="htmldemo"></div>
<style>
#htmldemo{
    height: 30px;
    width: 30px;
    background-color: #00aa9a;
    animation-name: moveX;
    animation-duration: 1s;
    animation-timing-function: linear;
    animation-iteration-count: infinite;
    animation-direction: alternate;
    animation-fill-mode : both;
}
@keyframes moveX {
    0%{
        transform: translateX(0px);
    }
    100%{
        transform: translateX(100px);
    }
}
</style>

```html
<div id="htmldemo"></div>
<style>
  #htmldemo {
    height: 30px;
    width: 30px;
    background-color: #00aa9a;
    animation-name: moveX;
    animation-duration: 1s;
    animation-timing-function: linear;
    animation-iteration-count: infinite;
    animation-direction: alternate;
    animation-fill-mode: both;
  }
  @keyframes moveX {
    0% {
      transform: translateX(0px);
    }
    100% {
      transform: translateX(100px);
    }
  }
</style>
```

## 插入 iframe

<iframe
  src="https://music.163.com/outchain/player?type=2&id=29732733&auto=0&height=66"
  frameborder="0"
  height="86px"
></iframe>

```html
<!-- 属性什么的不要错了，最好用双引号括住 -->
<!-- 网易云的iframe需要做些调整，调整如下 -->
<iframe
  src="https://music.163.com/outchain/player?type=2&id=29732733&auto=0&height=66"
  frameborder="0"
  height="86px"
></iframe>
```

[^1]: Here is the footnote 1 definition.

```
[^1]: Here is the footnote 1 definition.
```