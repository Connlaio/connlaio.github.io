---
title: RealVNC Server Ubuntu 18.04 无显示器连接
comments: true
toc: true
categories:
  - 杂项
date: 2022-07-06 11:55:15
tags:
---

  安装xserver-xorg-video-dummy，做一个虚拟的显示器。

```shell
sudo apt install xserver-xorg-video-dummy
```
  
  修改配置文件。

```shell
cd /usr/share/X11/xorg.conf.d
sudo vim xorg.conf
```

配置文件内容如下：
```shell
Section "Device"
    Identifier "DummyDevice"
    Driver "dummy"
    VideoRam 256000
EndSection
 
Section "Screen"
    Identifier "DummyScreen"
    Device "DummyDevice"
    Monitor "DummyMonitor"
    DefaultDepth 24
    SubSection "Display"
        Depth 24
        Modes "1920x1080_60.0"
    EndSubSection
EndSection
 
Section "Monitor"
    Identifier "DummyMonitor"
    HorizSync 30-70
    VertRefresh 50-75
    ModeLine "1920x1080" 148.50 1920 2448 2492 2640 1080 1084 1089 1125 +Hsync +Vsync
EndSection
```
重启即可。

**Warning** 经测试发现，在 18.04 上，如果使用此方式进行链接，会导致无法打开系统设置，或者点击系统设置后，

退出当前用户（杀死所有进程），并显示登录界面。无法判断是否为个例。