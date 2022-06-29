---
title: Ubuntu 解决耳机驱动异常
comments: true
toc: true
categories:
  - 杂项
date: 2022-6-29 10:09:00
tags: 日常软件
---

```shell
echo "options snd_hda_intel probe_mask=1" | sudo tee /etc/modprobe.d/tensorbook.conf

sudo update-initramfs -u -k all

```
重新启动即可。