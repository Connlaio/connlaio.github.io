---
title: 修改IiTunes备份路径
comments: true
toc: true
categories:
  - 杂项
date: 2018-09-17 20:26:08
tags: 日常软件
---

  一直用的是Windows PC，奈何手里有个iPhone，有时候想备份了，备份到电脑吧，iTunes 无脑存C盘，

  真是让人很无语。搞了个外置硬盘来存备份，网上搜罗了下设置方法，终于找到了。那就是软链接。
参见[如何在Windows或Linux创建链接](https://www.howtogeek.com/howto/16226/complete-guide-to-symbolic-links-symlinks-on-windows-or-linux/)


1. 在目标磁盘分区（例如H盘）下面创建一个名为“Backup”的文件夹。
2. 将 iTunes 默认备份路径（C:\Users\自己的电脑名\AppData\Roaming\Apple Computer\MobileSync\Backup）下的所有子文件夹（每个子文件夹代表着一个设备的备份目录）复制到刚才建立的 __"D:\Backup"__ 文件夹下面。
3. 删除原始的 C 盘路径下 Backup 文件夹及其下面的所有子文件夹。
4. 找到 C:\Windows\System32\cmd.exe，选择“以管理员身份运行”，打开命令提示符程序，并输入下面字符串命令并按回车执行：
```shell
　　mklink /D "C:\Users\自己电脑名\AppData\Roaming\Apple Computer\MobileSync\Backup" "D:\Backup"
```

　　上述命令执行完之后，会自动在 MobileSync 文件夹下建立一个名为 Backup 的镜像文件夹，即 H 盘下的 Backup 文件夹已经映射到原先默认备份路径了。

　　大功告成。

**注** 文中的方法参考了[威锋论坛中的帖子](https://bbs.feng.com/mobile-news-read-0-684174.html)。
