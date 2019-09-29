+++ 
draft = false
date = 2019-09-29T10:29:31+08:00
title = "How to customize Ubuntu ISO"
slug = "/cubic" 
tags = []
categories = []
+++

# How to customize Ubuntu ISO

[Cubic](https://launchpad.net/cubic) is a custom Ubuntu ISO creator. With it you can take a standard Ubuntu ISO image, install all the third-party software you need, and then create a custom, bootable (and installable) image from that. In the end, you'll have a Ubuntu platform that includes all the software you need, without having to add everything post-install.

## Install

```shell
sudo add-apt-repository ppa:cubic-wizard/release
sudo apt update
sudo apt install cubic
```

## Tutorial

1. Launch Cubic from your desktop menu and select a directory for your new project.
2. Select the ISO you want to customize.
3. On the next screen, Cubic displays an embedded terminal window with a chroot environment. This is where you can customize the ISO.

   - You can install extra packages, remove any installed packages or change configuration files in this command line chroot.

4. Choose which Linux kernel to use and packages from a typical install to remove.
5. Follow the wizard, it's done!

## Notice

- You can add any sources under `/etc/apt/sources.list.d/`

- If you failed at step: 'select and install software' when install with your customize ISO . You should check your `/etc/apt/` and `/etc/apt/sources.list.d` dirs and then **remove `*.save`** under both of them.
- `rm -rf /etc/apt/sources.list.d/*.save /etc/apt/*.save`
