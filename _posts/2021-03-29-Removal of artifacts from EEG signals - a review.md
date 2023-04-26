---
layout: post
title: "Removal of artifacts from EEG signals: a review"
tags: [Paper Notes, EEG]
description: Paper notes about "Removal of artifacts from EEG signals - a review".
mathjax: false
comment: true
---

介绍 EEG 基本特征，常见伪迹类型及其特征，常用伪迹去除方法及其优缺点。

---

> 原始论文：Jiang, X., Bian, G. B., & Tian, Z. (2019). [Removal of artifacts from EEG signals: a review](https://www.mdpi.com/1424-8220/19/5/987). Sensors, 19(5), 987.

## Background

### EEG 基本特征

EEG 频率范围一般 0.1Hz - 100Hz，可以分为五个频段，下面列了四种常见频段。

![image-20210406173110977](/assets/images/image-20210406173110977.png)

### 常见伪迹类型

这里主要指生理伪迹（区别于仪器误差），下面列出几种常见信号特征。

![image-20210406173928986](/assets/images/image-20210406173928986.png)

#### Ocular Artifacts

主要来自眼动 (eye movement) 和眨眼 (blink), 通常可被 EOG 记录下来。EOG 幅值一般比 EEG 大很多，但频率差不多。EEG 和 EOG 可能相互影响，因此去除 EOG 伪迹时这种双向干扰也可能会引入额外误差。

#### Muscle Artifacts

肌电来源比较复杂，只要是会让电极周围肌肉运动的动作都可能引发，比如说话、吞咽。可由 EMG 记录，频率分布比较广泛 (0Hz - > 200Hz)，由于肌肉动作的多样性，其伪迹较难检测和去除。但 EMG 和 EEG 在时空上有本质上的统计独立性 (statistical independence)，因此适合用 ICA 去除肌电伪迹。

#### Cardiac artifacts

电极放在血管附近时容易引入心脏伪迹，由心脏收缩扩张引起，也被称为脉冲伪迹 (pulse artifacts)，频率在 1.2Hz 左右，由于特征与 EEG 相近，很难去除。
另一种心脏输出的电信号可以被 ECG 记录下来，由于特征较为明显很容易去除。

## Single Artifacts Removal Techniques

### Regression Methods

传统去噪方法常用回归，它假定每个 channel 的数据是干净 EEG 数据和一系列伪迹信号的累加。这种方法首先需要伪迹信号的参考通道，通过参考通道跟 EEG 通道信号的幅值关系计算出 transmission factor，然后从原始信号中减掉伪迹即可。
缺点：需要好的回归参考通道，很多时候是不现实的。

### Wavelet Transform

把时域信号转换到时域和频域，相比于傅里叶变换具有更好的时频特性 (tunable time-frequency tradeoff; superiority of non-stationary signal analysis)。

### BBS (Blind Source Separation)

BBS 包括很多无监督算法，不需要先验信息和额外的参考通道。下面介绍一些常见算法：

#### PCA (Principal Component Analysis)

依靠 SVD 实现，比回归效率更高。
缺点：要求伪迹成分与 EEG 数据不相关

#### ICA (Independent Component Analysis)

最常用之一。假定信号源是大脑信号和伪迹信号的线性混合，并且可分离为不同的独立成分 (independent components, ICs)，分开后只需要去掉伪迹对应的 ICs 就可以重构出干净的 EEG 信号。

#### CCA (Canonical Correlation Analysis)

相比于 ICA，CCA 使用二阶统计量 (second order statistics) 缩短计算时间。CCA 从无关源中 (uncorrelated sources) 分离独立成分，而 ICA 从统计独立源 (statistical independent sources) 中分离成分。CCA 比 ICA 在 muscle artifacts 的去除上更有优势。

#### Source Imaging Based Method

EEG source imaging (ESI) is a model-based imaging technique that combines temporal and spatial components of EEG to ﬁnd the source of scalp-recorded potentials.

### EMD (Empirical Mode Decomposition)

某种信号处理方法。

### Filtering Methods

不同类型的滤波方法，比如 Adaptive Filtering、Wiener Filtering等。

## Hybrid Methods

各种方法的组合，比如 EMD-BSS、Wavelet-BSS、BSS and SVM (Support Vector Machine)等。

## 不同方法的比较

见图

![image-20210406230051091](/assets/images/image-20210406230051091.png)

## 结论

比较了不同技术的不同适应场景，很多技术都不太了解就不细写了，可能需要过些时间再看一遍。