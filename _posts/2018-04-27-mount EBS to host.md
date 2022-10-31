---
layout: post
title: 云主机挂载云硬盘
tags: [Linux]
description: 云主机挂载云硬盘
keywords: Linux, VPS, 云主机, 云硬盘
mathjax: false
comment: true
---

记载如何将云硬盘挂载到实验室主机，这样就不用每次都去搜教程了。

---

## 前提

云盘已经连接到云主机。

## 步骤

### 查看当前所有硬盘

找到你要挂载的硬盘名称

```bash
sudo fdisk -l
```

![查看所有硬盘](https://cloud.chenkun.pro/img/2018-04-27-01.png){:height="70%" width="70%"}_查看所有可用硬盘_

### 格式化

第一次挂载的时候需要对云盘进行格式化，之后挂载就不必格式化了，否则会清除云盘里的所有内容（这里的 `/dev/vdd` 为步骤 1 查看得到的实际路径，每次可能都不一样）。

```bash
sudo mkfs.ext4 /dev/vdd
```

![初始化云盘](https://cloud.chenkun.pro/img/2018-04-27-02.png){:height="70%" width="70%"}_初始化云盘_

### 挂载

创建一个目录作为该云盘的挂载点，假设目录名为 `qa_test`。将云盘 `/dev/vdd` 挂载到 `qa_test` 目录下。

```bash
sudo mount -t ext4 /dev/vdd /home/ubuntu/qa_test
```

挂载完成是没有提示的，可以通过如下命令查看。

```bash
df -h
```
效果：
![查看挂载结果](https://cloud.chenkun.pro/img/2018-04-27-03.png){:height="70%" width="70%"}_查看挂载结果_

### 更改目录所属组及所有者

挂载完之后挂载的目录所有者跟所属组都是 `root`，所以普通用户没法在它下面操作。
更改目录所有者及所属组：

```bash
sudo chown username qa_test
sudo chgrp groupname qa_test
```

`username` 及 `groupname` 分别改为自己的用户名和用户组名，`qa_test` 为待修改的目录名。
