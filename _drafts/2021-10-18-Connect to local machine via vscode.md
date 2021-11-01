---
layout: post
title: 使用 VSCode 通过跳板机免密连接内网服务器
tags: [Linux]
description: Connect to the local machine via VSCode
mathjax: false
comment: true
---

某些情况下两台机器 A，B 内网相通，但是只有 A 可以从外部直接访问，这时候可以通过 VSCode 间接连到 B。其实通过 ssh 中转也行，但是每次都这么弄很麻烦，而且走 ssh 不能直接操作 jupyter 文件。

---

**目录**

* toc
{:toc}

## VSCode 中 B 作为跳板连接 A

1. 安装 `Remote Development` 插件集合（只装 `Remote SSH` 也行）
2. vscode 中打开 ssh 配置文件
