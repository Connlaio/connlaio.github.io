---
title: U盘安装Linux系统后如何正常恢复容量
comments: true
toc: true
categories:
  - 杂项
date: 2022-6-29 10:09:00
tags: 日常软件
---


## 使用Windows 系统

1. 使用cmd
2. 键入 diskpart
3. list disk  And select the U disk (etc: select disk 1 )
4. clean
5. create partition primary
6. format fs fat32(比较耗时)