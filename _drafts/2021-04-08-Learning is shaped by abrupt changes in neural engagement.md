---
layout: post
title: Learning is shaped by abrupt changes in neural engagement
tags: [Neuroscience, Paper Notes]
description: Paper notes about "Learning is shaped by abrupt changes in neural engagement" in nature neuroscience
mathjax: false
comment: true
---

目前

---

**目录**

* TOC
{:toc}

> Paper link: [Learning is shaped by abrupt changes in neural engagement](https://www.nature.com/articles/s41593-021-00822-8)

## Abstract

　　在本文中，

## Model


![QANet 模型结构](https://cloud.chenkun.pro/img/2018-06-05-01.png)_图 1 QANet 模型结构_

### Input embedding layer


### Embedding Encoder Layer

　　由 encoder block 组成（这部分只用到了 1 个 encoder block），单个 encoder block 结构自底向上依次包含位置编码 (position encoding)，卷积 (conv) 层，self attention 层和前馈网络 (fnn) 层，具体参考图 1 右侧 one encoder block。每个基本操作 (conv/self-attention/ffn) 都放在残差块 (residual block) 里，具体各层参数参考原论文。原论文对这里卷积层和 self-attention 层的作用解释是：卷积能够捕获上下文局部结构，而 self-attention 则可以捕捉文本之间全局的相互作用。

![depthwise convolution](https://cloud.chenkun.pro/img/2018-06-05-02.png){:height="60%" width="60%"}_图 2 depthwise convolution_

　　然后是 pointwise convolution，和常规卷积运算类似，卷积核的尺寸为 1 * 1 * M，M 等于上一层的 depth。它会将上一步的 feature map 在深度方向上进行加权组合，生成新的 feature map，有几个 filter 就有几个 feature map。如图 3 所示，输入 depth 为 3，卷积核大小为 1 * 1 * 3，使用 4 个 filter，最终生成 4 个 feature map。

![pointwise convolution](https://cloud.chenkun.pro/img/2018-06-05-03.png){:height="60%" width="60%"}_图 3 pointwise convolution_

　　直观动画见 [这里](https://www.slideshare.net/DongWonShin4/depthwise-separable-convolution)。

**self-attention**：

　　先简单介绍下 Attention 机制。Attention 机制的基本原理如图 4 所示，对于给定的 Query，计算 Query 和各个 Key 之间的相似度（常用的相似度函数有点积，拼接，感知机等），得到每个 Key 对应的 Value 的权重，用 softmax 函数对权重进行归一化，最后将权重和对应的 Value 进行加权求和得到 Attention 数值。用公式表示就是：

$$
Attention(Q, K, V)=softmax(Similarity(Q, K)) * V
$$

　　对应到 NLP 任务里，可以将句子中的每一个单词看作一个 (Key, Value) 对，则原句子可表示为一系列 (Key,Value) 对。分别计算 Query 和每一个 Key 的相似性，得到每个 Key 对应的 Value 的权重，再对 Value 进行加权求和，得到 Attention 值。

![Attention 机制](https://cloud.chenkun.pro/img/2018-06-05-04.png){:height="60%" width="60%"}_图 4 Attention 机制_

　　目前在 NLP 任务中，Key 和 Value 一般都是同一个，即 Key=Value。而 self-attention 则更进一步，令 Q=K=V，使得对于输入的句子，它的任意两个单词之间都要计算 Attention，所以两个单词之间无论有多长距离，最大的路径长度也都只是 1 ，因此 self-attention 可以有效捕获长距离依赖关系，如图 5（来源于原论文附录部分）所示。另外 self-attention 不像 RNN 一样依赖于前一时刻的状态，因此它有利于并行计算。

![可视化 self-attention](https://cloud.chenkun.pro/img/2018-06-05-05.png){:height="60%" width="60%"}_图 5 可视化 self-attention_


## 原始论文：

[QANet: Combining Local Convolution With Global Self-Attention For Reading Comprehension](https://openreview.net/pdf?id=B14TlG-RW)[CMU & Google]

