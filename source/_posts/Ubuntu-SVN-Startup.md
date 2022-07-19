---
title: Ubuntu svn开机启动脚本
comments: true
toc: true
categories:
  - Linux
  - Shell
date: 2022-07-19 09:41:14
tags: [ Ubuntu,Linux,Shell,subversion ]
---

最近配置了一台svn服务器，参考了网上很多开机启动的配置，发现都没有生效。直到测试了其中一个脚本。内容如下：
```shell
#!/bin/sh
### BEGIN INIT INFO
# Provides:          start-svn.sh
# Required-Start:
# Required-Stop:
# Default-Start:     2 3 4 5
# Default-Stop:
# Short-Description: start svn service
### END INIT INFO

svnserve -d -r /home/xxx/localsvn
```

关键的部分是，注释说明部分，**一定** 要加上，不然，没有效果。

将这个脚本命名为 __start-svn.sh__ （或者其他你喜欢的名字）， 添加好可执行权限 （ __chmod +x start-svn.sh__ ），然后复制到 __/etc/init.d/__ 目录下。

之后在终端键入指令
```shell
sudo update-rc.d start-svn.sh defaults 100
```
之后重启。

检查svn 是否能够开机自启动
```shell
ps -aux | grep svn
```

如果没有，那么执行 __runlevel__ ，查看当前启动level, 会打印一个数字, 把这个数字加到脚本 start-svn.sh 的 Default-Start 后面, 然后再执行一遍试试。

参考链接:[ubuntu安装SVN并设置开机启动-爱代码爱编程](https://icode.best/i/59118544595426)