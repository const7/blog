---
layout: post
title: Ubuntu 安装 VNC server
tags: [Linux]
description: VNC server installation on Ubuntu
mathjax: false
comment: true
---

记录服务器上安装 VNC server 的流程跟配置，防止下次踩坑。

---

**目录**

* toc
{:toc}

> 当前版本: Ubuntu 20.04，默认桌面环境：GNOME

## 基本介绍

### VNC server

有多种 VNC server 可供选择，这里我们使用 [TigerVNC](https://tigervnc.org)。

### 桌面环境

用来显示图形界面的，在这里分两种，一是服务器本身的桌面环境，二是你想在 VNC 里使用的桌面环境。服务器本身可以安装多种桌面环境，使用 VNC 时需要在配置里指定其中一种。

目前 ubuntu 自带桌面环境只有 gnome，常见的环境包括 gnome，kde，xfce 等。xfce 相对比较轻量，但是丑，ubuntu 默认带了 gnome，所以这里我们选择 gnome 作为 vnc 的桌面环境。

## 安装

1. 安装 TigerVNC（会自动帮你安装其它依赖包）

    ```shell
    sudo apt install tigervnc-standalone-server
    ```

2. 设置登陆密码

    ```shell
    vncpasswd
    ```

    > Would you like to enter a view-only password (y/n)? **n**

## 配置

1. 创建配置文件并赋予执行权限

    ```shell
    touch ~/.vnc/xstartup
    chmod 775 ~/.vnc/xstartup
    ```

2. 写入配置文件

    将下面内容写入到 `~/.vnc/xstartup` 里：

    ```shell
    #!/bin/sh
    unset SESSION_MANAGER
    unset DBUS_SESSION_BUS_ADDRESS
    export PATH=/usr/bin:$PATH # promote priority of /usr/bin, aviod conda problem
    export XKL_XMODMAP_DISABLE=1
    export XDG_CURRENT_DESKTOP="GNOME-Flashback:GNOME"
    export XDG_MENU_PREFIX="gnome-flashback-"
    [ -x /etc/vnc/xstartup ] && exec /etc/vnc/xstartup
    [ -r $HOME/.Xresources ] && xrdb $HOME/.Xresources
    xsetroot -solid grey
    vncconfig -iconic &
    gnome-session --session=gnome-flashback-metacity --disable-acceleration-check &
    ```

    > 配置这里是最坑的，配置有问题很容易在启动后碰到黑屏灰屏等奇奇怪怪的状况，而且 log 基本没什么参考价值。关于配置文件一些细节跟注意事项可参考 [VNC server配置](https://segmentfault.com/a/1190000022707961)。

## 运行

### 启动

> 如果配置文件中没有添加 `export PATH=/usr/bin:$PATH`，启动 VNC 之前一定要退出 conda 的 `base` 或者任何虚拟环境，不然有可能某些包你装了但是提示找不到或者启动失败，详情可参考上述 VNC 配置链接。

```shell
vncserver
```

启动后会提示你目前用的是哪个端口，比如 `New 'xxxx' desktop at :2 ....` 即为 `5902`（默认从 `5900` 开始）。

如果启动后发现 VNC viewer 显示不正常可能是因为某些包没有安装，在 `~/.vnc/*.log` 里会看到提示，手动安装就行。`-localhost no` 表示允许非局域网用户访问，如果碰上学校网络坑爹可能需要加上这个。

### 其他问题

启动时碰到的一个报错 `vncserver: /usr/bin/Xtigervnc did not start up, please look into '/home/xxx/.vnc/yyy:1.log' to determine the reason! -2`，查看 `/home/xxx/.vnc/yyy:1.log` 发现是如下内容：

```bash
_XSERVTransSocketUNIXCreateListener: ...SocketCreateListener() failed
_XSERVTransMakeAllCOTSServerListeners: server already running
(EE)
Fatal server error:
(EE) Cannot establish any listening sockets - Make sure an X server isn't already running(EE)
```

解决办法：手动创建一个 X1 文件并设置权限，原理未知

```bash
mkdir /tmp/.X11-unix
touch /tmp/.X11-unix/X1
chmod 777 /tmp/.X11-unix/X1
```

### 常用命令

1. 指定端口启动，比如从 `5902` 端口启动

    ```shell
    vncserver :2
    ```

    `vncserver :3` 则为 `5903` 端口启动。

2. 关闭某个端口进程，比如 `5902`

    ```shell
    vncserver -kill :2
    ```

3. 指定分辨率（仅本次运行期间有效）

    ```shell
    vncserver -geometry 1920x1080
    ```