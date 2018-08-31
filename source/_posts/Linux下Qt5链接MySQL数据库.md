---
title: Linux下Qt5链接MySQL数据库
comments: true
toc: true
categories:
  - 编程
date: 2018-08-31 16:43:19
tags: Linux
---
Qt 访问 MySql 需要 2 个库文件，一个是 Qt 自己的 MySql 驱动插件，另一个是 MySql 的动态链接库，缺一不可。在 Qt 程序里加载 MySql 驱动插件，其依赖于MySql 的动态链接库。

在Windows下，使用Qt5链接MySql应该是一件比较简单的事，但是，Linux下却不见得如此。

背景介绍：

    系统环境：Ubuntu 14.04

    Qt版本： Qt 5.3.1 (GCC 4.6.1, 32 bit)

    MySQL版本：5.6

首先我通过官网下载了Qt的安装包，安装了Qt，安装目录为 _~/Qt5.3.1_，之后通过apt-get安装了MySql。然后使用了一个简单的程序进行MySql的链接测试，发现报错，错误为
```shell
QSqlDatabase: QMYSQL driver not loaded
QSqlDatabase: available drivers: QSQLITE QMYSQL QMYSQL3 QPSQL QPSQL7  false  
```

因为之前在Windows上也遇到了类似的报错，只要找到MySql的安装路径，将libmysql.dll动态库添加到Qt安装目录下的bin目录即可，在Windows上我的Qt的安装目录为 _C:\Qt\Qt5.3.1\5.3\mingw482_32\bin_ 。Linux下又该如何解决呢？

  是否是Linux下MySql库的问题，还是Qt当中MySQL驱动插件的问题？
通过命令行进入Qt的安装目录，
_connlaio@ubuntu:~/Qt5.3.1/5.3/gcc/plugins/sqldrivers$_  命令

```shell
ldd libqsqlmysql.so
libmysqlclient_r.so.16 => not found ----
```

说明 MySQL的动态链接库无法找到，但是，在进入 _/usr/lib/_ 目录下通过find命令发现，MySQL的动态链接库是存在的，不过，名字不同。
```shell
 find -name "libmysql*" -print
./i386-linux-gnu/libmysqlclient_r.so.18
```

看来，我们需要重新编译Qt的MySQL驱动插件。找到Qt源码，进入 _qtbase/src/sql/_ 目录，最简单的方法是将目录下的sql工程添加到Qt中，构建MySQL驱动。

```shell
sudo apt-get install libmysqlclient-dev
```

然后到 _/qtbase/plugins/sqldriver_ 目录下就能看到生成的动态库。然后将新的MySQL驱动插件拷贝到对应的 _gcc/plugins/sqldrivers_ 目录下即可。
