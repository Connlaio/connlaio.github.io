---
title: Ubuntu 18.04 安装 WineHQ版本
comments: true
toc: true
categories:
  - 杂项
date: 2022-6-29 10:09:00
tags: 日常软件
---

## 准备工作

1. 安装 libfaudio0 库

   ```shell
   cd ~/Desktop
   wget https://download.opensuse.org/repositories/Emulators:/Wine:/Debian/xUbuntu_18.04/amd64/libfaudio0_19.07-0~bionic_amd64.deb
   wget https://download.opensuse.org/repositories/Emulators:/Wine:/Debian/xUbuntu_18.04/i386/libfaudio0_19.07-0~bionic_i386.deb
   sudo dpkg -i libfaudio0_19.07-0~bionic_amd64.deb libfaudio0_19.07-0~bionic_i386.deb
   sudo apt --fix-broken install
   ```

## 开始

### 如果是 64位系统，需要使能32位架构

```shell
sudo dpkg --add-architecture i386 
```

### 添加key

```shell
wget -nc https://dl.winehq.org/wine-builds/winehq.key
sudo apt-key add winehq.key
```

### 添加对应的软件源

| For this version:                | Use this command:                                            |
| -------------------------------- | ------------------------------------------------------------ |
| Ubuntu 21.10                     | sudo add-apt-repository 'deb https://dl.winehq.org/wine-builds/ubuntu/ impish main' |
| Ubuntu 21.04                     | sudo add-apt-repository 'deb https://dl.winehq.org/wine-builds/ubuntu/ hirsute main' |
| Ubuntu 20.10                     | sudo add-apt-repository 'deb https://dl.winehq.org/wine-builds/ubuntu/ groovy main' |
| Ubuntu 20.04  / Linux Mint 20.x  | sudo add-apt-repository 'deb https://dl.winehq.org/wine-builds/ubuntu/ focal main' |
| Ubuntu 18.04  /  Linux Mint 19.x | sudo add-apt-repository 'deb https://dl.winehq.org/wine-builds/ubuntu/ bionic main' |

### 更新

```shell
sudo apt update
```

### 安装需要的版本

| Stable branch      | `sudo apt install --install-recommends winehq-stable ` |
| ------------------ | ------------------------------------------------------ |
| Development branch | `sudo apt install --install-recommends winehq-devel `  |
| Staging branch     | `sudo apt install --install-recommends winehq-staging` |


###  If you have previously used the distro packages, you will notice some differences in the WineHQ ones:

- Files are installed to /opt/wine-devel, opt/wine-stable, or /opt/wine-staging (depending on which version you installed).

- Menu items are not created for Wine's builtin programs (winecfg, etc.), and if you are upgrading from a distro package that had added them, they will be removed. You can recreate them yourself using your menu editor.

- Binfmt_misc registration is not added. Consult your distro's documentation for [update-binfmts](http://manpages.ubuntu.com/manpages/jaunty/man8/update-binfmts.8.html) if you wish to do this manually.

- WineHQ does not at present package wine-gecko or wine-mono. When creating a new wine prefix, you will be asked if you want to download those components. For best compatibility, it is recommended to click Yes here. If the download doesn't work for you, please follow the instructions on the [Gecko](http://wiki.winehq.org/Gecko) and [Mono](http://wiki.winehq.org/Mono) wiki pages to install them manually.

- Beginning with Wine 5.7, the WineHQ Ubuntu packages have an optional debconf setting to enable CAP_NET_RAW to allow applications that need to send and receive raw IP packets to do so. This is disabled by default because it carries a potential security risk, and the vast majority of applications do not need that capability. Users of applications that do need it can enable CAP_NET_RAW after installing Wine by running

  ```shell
  dpkg-reconfigure wine-<branch>-amd64 wine-<branch> wine-<branch>-i386
  ```


##  无网络安装

  To install Wine on an Ubuntu machine without internet access, you must have access to a second Ubuntu machine (or VM) with an internet connection to download the Wine .deb package and its dependencies.

On the machine with internet, add the WineHQ repository and run apt update as described above.

Next, cache just the packages necessary for installing wine, without extracting them:

```shell
sudo apt-get clean
sudo apt-get --download-only install winehq-devel
sudo apt-get --download-only dist-upgrade
```

Copy all of the .deb files in /var/cache/apt/archives to a USB stick:

```shell
cp -R /var/cache/apt/archives/ /media/usb-drive/deb-pkgs/
```

Finally, on the machine without internet, install all of the packages from the flash drive:

```shell
cd /media/usb-drive/deb-pkgs
sudo dpkg -i *.deb
```

The same instructions can also be used for an offline installation of the `winehq-staging` packages.



参考链接：

[WineHQ]: https://wiki.winehq.org/Ubuntu



