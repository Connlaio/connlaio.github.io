---
title: Thinkpad e480 ubuntu 18.04 wifi adapter not found
comments: true
toc: true
tags: [Qt,Linux]
categories: 编程
date: 2018-09-28 12:19:48
---

  公司新配的E480 原装了win10，奈何有时候还要用Linux，系统内存只有8G，开虚拟机有些吃紧，索性直接装 双系统吧。

安装过程暂且不表，但是，装完后发现，18.04 下竟然没有 WiFi和蓝牙。Google一番，
发现了解决方案。

```shell
sudo apt update
sudo apt install build-essential git dkms
git clone https://github.com/tomaspinho/rtl8821ce.git
cd rtl8821ce
sudo ./dkms-install.sh
```
最后键入
```shell
sudo modprobe 8821ce
```

问题恰恰出在了这儿。之前所有的步骤都ok，或者通过
```shell
make
sudo make install
```
的方式，都能够顺利生成 __8821ce.ko__ 文件。但是，加载就是失败。报错为 _key is not available_ ， 大意如此。之后只能又遍寻方法，最后通过 关闭 BIOS 中的 Security Boost，重启之后，终获成功。

**Note** ： 安装时，一定要将 BIOS 中的 Security Boost 选项 设置为 关闭（disable）。
