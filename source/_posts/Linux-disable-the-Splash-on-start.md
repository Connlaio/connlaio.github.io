---
title: Linux 禁止开机动画(Plymouth)
comments: true
toc: true
categories:
  - 编程
date: 2018-08-31 11:07:45
tags: Linux
---

先介绍下背景。客户使用的公司产品是Debian系统的，老版本的软件中，使用了带有公司logo的开机动画，客户想要去掉。

因为这些软件和系统的配置都不是我经手的，恰恰接到了这个任务，只好Google+Baidu了。开机动画是Plymouth 定制的主题。先按照这个方向去查找。
无果。好吧，意外找到了一个帖子，同样的需求，好消息是，题主解决了问题。

大概思路是修改grub的启动配置。

上解决方法。

 **Note** : 以下操作，均使用的是 root 权限，或者 使用 sudo。

进入 __/etc/default/grub__ 目录

修改内容
```shell
GRUB_CMDLINE_LINUX_DEFAULT=”quiet splash”
```

将值修改为 __no splash__ .

**关键** : 执行命令。

``` shell
update grub
```

重启。
