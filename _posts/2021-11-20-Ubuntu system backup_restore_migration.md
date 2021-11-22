---
layout: post
title: Ubuntu 系统备份/恢复/迁移
tags: [Linux]
description: Ubuntu system backup/resotre/migration
mathjax: false
comment: true
---

本文中 **系统备份/恢复** 主要指本机不更换硬件情况下的备份/恢复，**迁移** 这里指把系统从本机迁移到不同配置的机器上。另一种常见场景是电脑更换硬盘后想把系统迁移到新的硬盘，直接全盘克隆就行。本文主要介绍系统迁移，系统备份/恢复可以算是迁移操作的子集。

---

> 本文基本参考 [ubuntu系统迁移与备份](https://www.jianshu.com/p/b8e44772b9f1)，对一些细节和错误进行添加和修正。

**目录**

* toc
{:toc}

## 基本环境

本文所有操作基于以下环境，不同系统/版本等在一些文件/命令上可能会有少许差异：

> 系统版本：Ubuntu 20.0.4
> 
> 引导方式：UEFI + GPT
>
> PS: 源机器中`home`、`boot` 等目录未单独分区，如果有单独分区则分别处理不同分区

## 前言

### 前置知识

Ubuntu 中遵循一切皆文件的宗旨，所以备份和迁移都是靠文件复制粘贴实现的，主要问题在于需要哪些文件不需要哪些文件。

从文件的角度看，`/` 目录下的所有文件构成了 Ubuntu **系统**。而从更底层一些的硬件（这里指硬盘）来看，完整的 Ubuntu 系统由 **系统** 和 **引导程序** 两部分组成。**系统** 存放 Ubuntu 的系统文件，即 `/` 目录下的内容，而 **引导程序** 用于启动时引导计算机在硬盘里找到 **系统** 所在位置。通常这俩部分分别在硬盘的两个分区里，通过 `fdisk -l` 命令可以查看分区信息，如图 1 所示：

![图 1 Ubuntu 硬盘分区](https://cloud.chenkun.pro/img/20211122155402.png)_图 1 Ubuntu 硬盘分区_

这里系统和引导处于同一块硬盘的两个不同分区里，蓝色框里是分区名，绿色框里是分区类型。EFI 代表引导程序分区，这里为 `/dev/nvme0n1p1`，而系统位于 `/dev/nvme0n1p2` 分区。

除了系统环境、软件等内容外，**系统** 中还包括：

- `/etc/fstab`, `/etc/fstab.d`（可能没有）: 启动文件
- `/boot/grub/grub.cfg`: 分区文件

### 原理

- **系统备份**：备份的是 **系统** 这个部分，即 `/` 目录下的内容，恢复也只需要恢复备份的 `/` 文件夹。

- **系统迁移**：涉及硬件上的改变，不能直接复制粘贴 `/` 下的内容。对于 **系统**，跟特定硬件相关的文件肯定不能直接复制到新系统。而对于 **引导程序**，由于硬盘变了，分区信息等也改变了，如果直接复制粘贴会导致计算机启动时找不到 **系统** 所在位置。所以系统迁移一般做法是先在新机器上按正常流程安装系统（使得 EFI System、fstab 启动文件以及 grub.cfg 分区文件都是匹配的），然后再把旧机器中部分文件替换到新机器中。

### 准备工作

> 假设把旧机器上的系统 **A** 备份恢复/迁移为新机器上的系统 **B**。

准备一个 Ubuntu 启动盘（推荐 [Ventoy](https://www.ventoy.net/cn/index.html)），再准备一个用来保存备份的移动硬盘（建议）。也可以把备份直接存到 U 盘，但需要注意容量以及文件系统格式（FAT32 无法保存单个 4GB 以上的文件）。

## 系统备份（在 A 上操作）

1. 使用 Ubuntu 启动盘在旧机器上进入 live CD 模式，即 *try ubuntu*，使用 root 权限进行后续操作（`sudo -i`）。

2. 使用 `fdisk` 命令查看硬盘分区，找到 A **系统** 所在分区（参考图 1），我这里为 `/dev/nvme0n1p2`。顺便看看移动硬盘所在分区，这里为 `/dev/sdb2`。

3. 挂载移动硬盘和 A 系统

    1. 进入根目录 `/`
    2. `mkdir data`: 创建 `data` 文件夹用于挂载移动硬盘
    3. `mount /dev/sda2 /data`: 挂载移动硬盘到 `data` 目录
    4. `mount /dev/nvme0n1p2 /mnt`: 挂载 A 系统到 `/mnt` 目录

4. 通过 `tar` 将需要迁移的文件备份到移动硬盘

    ```shell
    tar -cvpzf /data/ubuntu.backup --exclude=/mnt/proc --exclude=/mnt/lost+found --exclude=/mnt/tmp --exclude=/mnt/run --exclude=/mnt/media --exclude=/mnt/mnt /mnt
    ```
    > `tar` 的 exclude 参数有位置要求，需要根据 `tar` 版本来选择合适的放置位置（不同版本要求可能是相反的，这里是 1.3，需要把 exclude 放在待压缩目录前面）

    **参数说明**：

    - -c：新建一个备份文档
    - -v：显示详细信息
    - -p：保存权限，并应用到所有文件
    - -z：用 gzip 压缩备份文档
    - -f：指定备份文件的路径
    - \-\-exclude：排除指定目录

    **排除文件说明**：

    > 这里把 live CD 里的 `/mnt` 看作根目录，因为整个系统 A 都挂载在 `/mnt` 下。

    - `/proc`：一个虚拟文件系统，系统运行的每一个进程都会自动在这个目录下面创建一个进程目录。既然是系统自动创建，也就没必要备份的必要了。
    - `/tmp`：一个临时文件夹，系统的一些临时文件会放在这里。
    - `/lost+found`：系统发生错误时（比如非法关机），可以在这里找回一些丢失文件。
    - `/media`：多媒体挂载点，像u盘、移动硬盘、windons分区等都会自动挂载到这个目录下。
    - `/mnt`：临时挂载点，你可以自己挂载一些文件系统到这里。
    - `/run`：系统从启动以来产生的一些信息文件。

4. 解除移动硬盘和系统 A 的挂载

    ```shell
    umount /data
    umount /mnt
    ```

## 系统恢复/迁移（在 B 上操作）

### 系统恢复

如果是在旧机器上恢复备份，直接通过 live CD 把备份解压到 `/` 目录中然后手动创建之前排除的几个文件夹即可，一些操作细节可参考系统迁移步骤。

### 系统迁移

1. 使用 Ubuntu 启动盘正常安装 Ubuntu，然后同上在 live CD 中查看新机器上 **系统** B 所在分区和移动硬盘分区，这里刚好也分别为 `/dev/nvme0n1p2` 和 `/dev/sda2`.

    > PS: 新系统 EFI 分区在安装系统时已经自动配置好了，不需要我们额外修改
    > 
    > PPS: 虽然新机器上 **系统** B 的分区名跟 A 一样，但对应的 UUID 肯定是不同的，所以还需要额外配置，使得新系统能够正常启动

2. 挂载移动硬盘和 B 系统

    1. 进入根目录 `/`
    2. `mkdir data`: 创建 `data` 文件夹用于挂载移动硬盘
    3. `mount /dev/sda2 /data`: 挂载移动硬盘到 `data` 目录
    4. `mount /dev/nvme0n1p2 /mnt`: 挂载 B 系统到 `/mnt` 目录

3. 备份 B 的启动文件和分区文件

    ```shell
    cp /mnt/etc/fstab /data # 备份启动文件。若有fstab.d，也一并复制
    cp /mnt/boot/grub/grub.cfg /data # 备份分区文件
    ```

4. 系统迁移

    1. 把旧系统 A 迁移到新系统 B 中

        ```shell
        rm -rf /mnt/* # 删除新系统 B 的内容
        tar -xvpzf /data/ubuntu.backup -C /mnt # 将旧系统 A 的备份解压到新系统 B 中，注意这里的 `/mnt` 其实就是系统 B 的 `/` 目录
        ```

    2. 恢复 B 的启动文件和分区文件

        ```shell
        cp /data/fstab(fstab.d) /mnt/etc # 恢复启动文件
        cp /data/grub.cfg /mnt/boot/grub # 恢复分区文件
        ```

5. 使用 `boot-repair` 软件修复引导

    1. 安装 boot-repair

        ```shell
        sudo add-apt-repository ppa:yannubuntu/boot-repair && sudo apt update
        sudo apt install boot-repair
        ```
    2. 直接在命令行中输入 `boot-repair` 打开软件，使用 `Recommended repair` 一路默认选项确定就行，这里软件会自动帮你创建前面排除的那些文件夹，不需要再手动创建

6. 解除移动硬盘和系统 B 的挂载

    ```shell
    umount /data
    umount /mnt
    ```

7. 重启，完事