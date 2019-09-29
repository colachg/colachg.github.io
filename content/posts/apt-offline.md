+++ 
draft = true
date = 2019-09-29T10:29:40+08:00
title = "Mange packages on offline Debian system"
slug = "/apt-offline" 
tags = []
categories = []
+++

# How to manage packages on an off-line Debian system

There are plenty of off-line Debian systems just waiting to update index files, upgrade or install additional packages. This can be easily achieved by using apt-offline utility. I will shortly describe common usage scenario as it is very handy application.

## Installation

[apt-offline](https://pkgs.org/download/apt-offline) as the name says, apt-offline is an Offline APT Package Manager for APT based systems like Debian and Debian derived distributions such as Ubuntu, Linux Mint. Using apt-offline, we can fully update/upgrade our Debian box without having to connect it to the Internet. It is cross-platform tool written in the Python Programming Language and has both CLI and graphical interfaces.

`apt-cache depends apt-offline` shows that it depends on [apt, less, python3-magic], so you show download the packages to install it offline.`less` is often install by default.

`apt install --download-only pkg-name` will download pkg with it dependencies in `/var/cache/apt/archives`

## **Important notice**

- `apt-offline` uses `/etc/apt/trusted.gpg` file as keyring instead of files in the `/etc/apt/trusted.gpg.d/` directory.
- `sudo apt-key exportall | sudo gpg --no-default-keyring --import --keyring /etc/apt/trusted.gpg`
  `

## apt update

1. off-line system

   Add source under `/etc/apt/sources.list.d/` if you need.

   > `off-line$ sudo apt-offline set debian-update.sig --update`

2. on-line system

   Move generated signature file debian-update.sig to the on-line Debian system.

   > `on-line$ apt-offline get debian-update.sig --bundle debian-update.zip --threads 2`

3. off-line system

   > `off-line$ sudo apt-offline install debian-update.zip`

## apt upgrade

1. off-line system

   > `off-line$ sudo apt-offline set debian-upgrade.sig --upgrade`

2. on-line system

   Upload debian-upgrade.sig to the on-line Debian system and download required files.

   > `apt-offline get debian-upgrade.sig --bundle debian-upgrade.zip --threads 3`

3. off-line system  
   Upload debian-upgrade.zip bundle to the off-line system and install it using apt-offline utility to update APT database.

   > `off-line$ sudo apt-offline install debian-upgrade.zip`  
   > `off-line$ sudo apt-get upgrade`

## apt install pkg

1. off-line system

   > `off-line$ apt-offline set debian-install.sig --install-packages pkg1 pkg2 ...`

2. on-line system
   Upload debian-install.sig to the on-line Debian system and download required files.

   > `on-line$ apt-offline get debian-install.sig --bundle debian-install.zip`

3. off-line system
   Upload debian-install.zip file to the off-line Debian system, install it using apt-offline utility to update APT database.

   > `off-line$ sudo apt-offline install debian-install.zip`

   Now you can install specified packages using regular apt-get utility.

   > `off-line$ sudo apt-get install pkg1 pkg2`
