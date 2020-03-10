---
layout: post
title: A Dynamic Neural Network with Feedback for Trajectory Generation
categories: [Machine Learning, Paper Notes]
description: A Dynamic Neural Network with Feedback for Trajectory Generation论文翻译兼部分解读
keywords: Neural Networks, Dynamic Neural Networks, Trajectory planning, Path planning
mathjax: true
comment: true
---

## Abstract

本文详细介绍了由递归神经网络 (RNN) 和两个前馈神经网络 (FFNN) 组成的动态神经网络 (DNN)。该神经动力学系统 (neuro-dynamic system) 用于自适应地解决轨迹生成的问题。规划一条连接点轨迹的路径作为时间的函数。RNN 被设计用来展现所需的动态行为，该行为会让输出层 FFNN 生成特定的轨迹。为了使系统可自适应，引入了反馈 FFNN 以根据输出 FFNN 的输出生成 RNN 的输入。为生成不同类型的轨迹而进行的仿真显示了 DNN 在解决轨迹生成问题时具有反馈的功能。

## Problem

### Problem Description

需要以一种自适应的方法生成轨迹 $\mathbf{y}(t) \in {\mathbb{R}^n}$，因此下一个轨迹点必须依赖于当前轨迹点，即：

$$
\mathbf{y}(t) = \mathit{f}(\mathbf{y}(t), \alpha, \beta, ...)
$$

其中 $t_1 \gt t_0$，$\mathit{f}(·)$ 是非线性激活函数，$\alpha, \beta \in \mathbb{R}$ 是轨迹生成过程中的其他参数。

### Proposed Solution

这个问题可以分解为两步：首先生成一个中间轨迹 $\mathbf{o}(t)$，它是时间的函数。这个轨迹包含所需的时间特征，例如频率。第二步则对前面得到的轨迹进一步处理使之具有所需的空间特征，例如圆，椭圆或者数字8。这两个连续步骤可以用一个 DNN 来完成。RNN 通过映射 {$\mathbf{h}(·): [0, T] \to {\mathbb{R}^k}$} 生成随时间变化的轨迹，