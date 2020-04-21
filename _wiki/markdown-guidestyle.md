---
layout: wiki
title: Markdown 编写规范
tags: [Markdown]
description: Markdown 常用语法示例。
keywords: Markdown
mathjax: false
---

来源：

- [fex-team/styleguide](https://github.com/fex-team/styleguide)
- [mzlogin/chinese-copywriting-guidelines](https://github.com/mzlogin/chinese-copywriting-guidelines)

-----

**目录**

* TOC
{:toc}

## 说明

文档中使用的关键字「MUST」,「MUST NOT」,「REQUIRED」,「SHALL」,「SHALL
NOT」,「SHOULD」,「SHOULD NOT」,「RECOMMENDED」,「MAY」和「OPTIONAL」在 [RFC2119](http://oss.org.cn/man/develop/rfc/RFC2119.txt) 中有说明。

**还未定稿，对规范中提及的点有不赞同的欢迎 [提出 issues](https://github.com/fex-team/styleguide/issues/new)（请添加 `markdown` 标签）讨论。**

## 规则

* 后缀必须「MUST」使用 `.md`。
* 文件名必须「MUST」使用小写，多个单词之间使用`-`分隔。
* 文件编码必须「MUST」用 UTF-8。
* 文档标题应该「SHOULD」这样写。

    ```
    Markdown 编写规范
    ==========================
    ```
* 章节标题必须「MUST」以 `##` 开始，而不是 `#`。
* 章节标题必须「MUST」在 `#` 后加一个空格，且后面没有 `#`。

    ```
    // bad
    ##章节1

    // bad
    ## 章节1 ##

    // good
    ## 章节1
    ```

* 章节标题和内容间必须「MUST」有一个空行。

    ```
    // bad
    ## 章节1
    内容
    ## 章节2

    // good
    ## 章节1

    内容

    ## 章节2
    ```

* 代码段的必须「MUST」使用 Fenced code blocks 风格，如下所示：

        ```
        console.log("");
        ```

* 表格的写法应该「SHOULD」参考 [GFM](https://help.github.com/articles/github-flavored-markdown)，如下所示：

    ```
    First Header  | Second Header
    ------------- | -------------
    Content Cell  | Content Cell
    Content Cell  | Content Cell

    | Left-Aligned  | Center Aligned  | Right Aligned |
    | :------------ |:---------------:| -----:|
    | col 3 is      | some wordy text | $1600 |
    | col 2 is      | centered        |   $12 |
    | zebra stripes | are neat        |    $1 |
    ```

* 中英文混排应该「SHOULD」采用如下规则：
    - 英文和数字使用半角字符
    - 中文文字之间不加空格
    - 中文文字与英文、阿拉伯数字及 @ # $ % ^ & * . ( ) 等符号之间加空格
    - 中文标点之间不加空格
    - 中文标点与前后字符（无论全角或半角）之间不加空格
    - 如果括号内有中文，则使用中文括号
    - 如果括号中的内容全部都是英文，则使用半角英文括号 
    - 当半角符号 / 表示「或者」之意时，与前后的字符之间均不加空格
    - 其它具体例子推荐 [阅读这里](https://github.com/sparanoid/chinese-copywriting-guidelines)

* 中文符号应该「SHOULD」使用如下写法：
    - 用直角引号（「」）代替双引号（“”），不同输入法的具体设置方法请 [参考这里](http://www.zhihu.com/question/19755746)
    - 省略号使用「……」，而「。。。」仅用于表示停顿
    - 其它可以参考 [知乎规范](http://www.zhihu.com/question/20414919)

* 表达方式，应当「SHOULD」遵循《The Element of Style》：
    * 使段落成为文章的单元：一个段落只表达一个主题
    * 通常在每一段落开始要点题，在段落结尾要扣题
    * 使用主动语态
    * 陈述句中使用肯定说法
    * 删除不必要的词
    * 避免连续使用松散的句子
    * 使用相同的结构表达并列的意思
    * 将相关的词放在一起
    * 在总结中，要用同一种时态（这里指英文中的时态，中文不适用，所以可以不理会）
    * 将强调的词放在句末

## 扩展阅读

* Google 后来也出了 [Markdown 规范](https://github.com/google/styleguide/blob/gh-pages/docguide/style.md)，很多和这里是一样的，但也增加了一些约定，可以参考

## 中文排版参考

### 空格

#### 中英文之间需要增加空格

正确：

> 在 LeanCloud 上，数据存储是围绕 `AVObject` 进行的。

错误：

> 在LeanCloud上，数据存储是围绕`AVObject`进行的。

> 在 LeanCloud上，数据存储是围绕`AVObject` 进行的。

完整的正确用法：

> 在 LeanCloud 上，数据存储是围绕 `AVObject` 进行的。每个 `AVObject` 都包含了与 JSON 兼容的 key-value 对应的数据。数据是 schema-free 的，你不需要在每个 `AVObject` 上提前指定存在哪些键，只要直接设定对应的 key-value 即可。

例外：「豆瓣FM」等产品名词，按照官方所定义的格式书写。

#### 中文与数字之间需要增加空格

正确：

> 今天出去买菜花了 5000 元。

错误：

> 今天出去买菜花了 5000元。

> 今天出去买菜花了5000元。

#### 数字与单位之间无需增加空格

正确：

> 我家的光纤入户宽带有 10Gbps，SSD 一共有 10TB。

错误：

> 我家的光纤入户宽带有 10 Gbps，SSD 一共有 20 TB。

另外，度／百分比与数字之间不需要增加空格：

正确：

> 今天是 233° 的高温。

> 新 MacBook Pro 有 15% 的 CPU 性能提升。

错误：

> 今天是 233 ° 的高温。

> 新 MacBook Pro 有 15 % 的 CPU 性能提升。

#### 全角标点与其他字符之间不加空格

正确：

> 刚刚买了一部 iPhone，好开心！

错误：

> 刚刚买了一部 iPhone ，好开心！

#### `-ms-text-autospace` to the rescue?

Microsoft 有个 [`-ms-text-autospace`](http://msdn.microsoft.com/en-us/library/ie/ms531164(v=vs.85).aspx) 的 CSS 属性可以实现自动为中英文之间增加空白。不过目前并未普及，另外在其他应用场景，例如 OS X、iOS 的用户界面目前并不存在这个特性，所以请继续保持随手加空格的习惯。

### 标点符号

#### 不重复使用标点符号

正确：

> 德国队竟然战胜了巴西队！

> 她竟然对你说「喵」？！

错误：

> 德国队竟然战胜了巴西队！！

> 德国队竟然战胜了巴西队！！！！！！！！

> 她竟然对你说「喵」？？！！

> 她竟然对你说「喵」？！？！？？！！

### 全角和半角

不明白什么是全角（全形）与半角（半形）符号？请查看维基百科词条『[全角和半角](http://zh.wikipedia.org/wiki/%E5%85%A8%E5%BD%A2%E5%92%8C%E5%8D%8A%E5%BD%A2)』。

#### 使用全角中文标点

正确：

> 嗨！你知道嘛？今天前台的小妹跟我说「喵」了哎！

> 核磁共振成像（NMRI）是什么原理都不知道？JFGI！

错误：

> 嗨! 你知道嘛? 今天前台的小妹跟我说 "喵" 了哎!

> 嗨!你知道嘛?今天前台的小妹跟我说"喵"了哎!

> 核磁共振成像 (NMRI) 是什么原理都不知道? JFGI!

> 核磁共振成像(NMRI)是什么原理都不知道?JFGI!

#### 数字使用半角字符

正确：

> 这件蛋糕只卖 1000 元。

错误：

> 这件蛋糕只卖 １０００ 元。

例外：在设计稿、宣传海报中如出现极少量数字的情形时，为方便文字对齐，是可以使用全角数字的。

#### 遇到完整的英文整句、特殊名词，其內容使用半角标点

正确：

> 乔布斯那句话是怎么说的？「Stay hungry, stay foolish.」

> 推荐你阅读《Hackers & Painters: Big Ideas from the Computer Age》，非常的有趣。

错误：

> 乔布斯那句话是怎么说的？「Stay hungry，stay foolish。」

> 推荐你阅读《Hackers＆Painters：Big Ideas from the Computer Age》，非常的有趣。

### 名词

#### 专有名词使用正确的大小写

大小写相关用法原属于英文书写范畴，不属于本 wiki 讨论內容，在这里只对部分易错用法进行简述。

正确：

> 使用 GitHub 登录

> 我们的客户有 GitHub、Foursquare、Microsoft Corporation、Google、Facebook, Inc.。

错误：

> 使用 github 登录

> 使用 GITHUB 登录

> 使用 Github 登录

> 使用 gitHub 登录

> 使用 gｲんĤЦ8 登录

> 我们的客户有 github、foursquare、microsoft corporation、google、facebook, inc.。

> 我们的客户有 GITHUB、FOURSQUARE、MICROSOFT CORPORATION、GOOGLE、FACEBOOK, INC.。

> 我们的客户有 Github、FourSquare、MicroSoft Corporation、Google、FaceBook, Inc.。

> 我们的客户有 gitHub、fourSquare、microSoft Corporation、google、faceBook, Inc.。

> 我们的客户有 gｲんĤЦ8、ｷouЯƧquﾑгє、๓เςг๏ร๏Ŧt ς๏гק๏гคtเ๏ภn、900913、ƒ4ᄃëв๏๏к, IПᄃ.。

注意：当网页中需要配合整体视觉风格而出现全部大写／小写的情形，HTML 中请使用标准的大小写规范进行书写；并通过 `text-transform: uppercase;`／`text-transform: lowercase;` 对表现形式进行定义。

#### 不要使用不地道的缩写

正确：

> 我们需要一位熟悉 JavaScript、HTML5，至少理解一种框架（如 Backbone.js、AngularJS、React 等）的前端开发者。

错误：

> 我们需要一位熟悉 Js、h5，至少理解一种框架（如 backbone、angular、RJS 等）的 FED。

### 争议

以下用法略带有个人色彩，既：无论是否遵循下述规则，从语法的角度来讲都是**正确**的。

#### 链接之间增加空格

用法：

> 请 [提交一个 issue](#) 并分配给相关同事。

> 访问我们网站的最新动态，请 [点击这里](#) 进行订阅！

对比用法：

> 请[提交一个 issue](#) 并分配给相关同事。

> 访问我们网站的最新动态，请[点击这里](#)进行订阅！

#### 简体中文使用直角引号

用法：

> 「老师，『有条不紊』的『紊』是什么意思？」

对比用法：

> “老师，‘有条不紊’的‘紊’是什么意思？”

### 工具

| 仓库                                                                                                                            | 语言            |
|---------------------------------------------------------------------------------------------------------------------------------|-----------------|
| [vinta/paranoid-auto-spacing](https://github.com/vinta/paranoid-auto-spacing)                                                   | JavaScript      |
| [huei90/pangu.node](https://github.com/huei90/pangu.node)                                                                       | Node.js         |
| [huacnlee/auto-correct](https://github.com/huacnlee/auto-correct)                                                               | Ruby            |
| [sparanoid/space-lover](https://github.com/sparanoid/space-lover)                                                               | PHP (WordPress) |
| [nauxliu/auto-correct](https://github.com/NauxLiu/auto-correct)                                                                 | PHP             |
| [hotoo/pangu.vim](https://github.com/hotoo/pangu.vim)                                                                           | Vim             |
| [sparanoid/grunt-auto-spacing](https://github.com/sparanoid/grunt-auto-spacing)                                                 | Node.js (Grunt) |
| [hjiang/scripts/add-space-between-latin-and-cjk](https://github.com/hjiang/scripts/blob/master/add-space-between-latin-and-cjk) | Python          |
