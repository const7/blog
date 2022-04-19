---
layout: post
title: Hash your password when connecting through wired 802.1x authentication
tags: [Linux]
description: Hash your wifi/wired network password in ubuntu.
mathjax: false
comment: true
---

`netplan` has been the default network configuration tool since Ubuntu 20. Several hash solutions based on `netplan` didn't work in practice, so I switched to the `ifupdown` for configuration.

---

**TOC**

* toc
{:toc}

> Current os version: Ubuntu 20.04 focal

## Step 1: Install ifupdown

```bash
sudo apt install ifupdown net-tools
```

## Step 2: Configuration

1. `wpa_supplicant` configuration for the network.

    ```bash
    ifconfig    # find your network device name, just find the one you want to use. For me it's "enp0s31f6"
    echo -n plaintext_password_here | iconv -t UTF-16LE | openssl md4	# generate hashed password (you need to use appropriate hash method to generate. md4 is corresponding to MSCHAPV2 authentication in phase2)
    ```

    Create and edit the configuration file `wpa_supplicant_enp0s31f6.conf` (any file name you want) under the `/etc/wpa_supplicant` folder. Replace your hased password (in the form of `hash:your_hashed_password`) and identity.

    ```bash
    network={
        key_mgmt=IEEE8021X
        eap=PEAP
        identity="****"
        password=hash:bc67...8c
        phase2="autheap=MSCHAPV2"
    }
    ```

2. `interface` configuration

    Edit the `/etc/network/interface` as below, remember to replace the network device name and wpa configuration file.

    ```bash
    # Include files from /etc/network/interfaces.d:
    source-directory /etc/network/interfaces.d

    # the loopback network interface
    auto lo
    iface lo inet loopback

    # the primary network interface
    allow-hotplug enp0s31f6
    iface enp0s31f6 inet dhcp

    wpa-driver wired
    wpa-conf /etc/wpa_supplicant/wpa_supplicant_enp0s31f6.conf
    ```

## Step 3: Disable current network manager

Disable any services related to `systemd-networkd` and `NetworkManager`, like:

```bash
sudo systemctl stop NetworkManager.service
sudo systemctl disable NetworkManager.service

sudo systemctl stop systemd-networkd.socket
sudo systemctl disable systemd-networkd.socket

sudo systemctl stop systemd-networkd.service
sudo systemctl disable systemd-networkd.service
```

## Step 4: Reboot

Reboot and check whether it can automatically connect to the network.

### Reference

1. [How to switch back networking to /etc/network/interfaces on Ubuntu 20.04 Focal Fossa Linux
](https://linuxconfig.org/how-to-switch-back-networking-to-etc-network-interfaces-on-ubuntu-20-04-focal-fossa-linux)

2. [Manual of 802.1x Authentication for Wired Access (CUHK)](https://itso.cuhk.edu.cn/sites/itso.prod.dpsite02.cuhk.edu.cn/files/2021-08/Manual%20of%20802.1x%20Authentication%20for%20Wired%20Access%20.pdf)