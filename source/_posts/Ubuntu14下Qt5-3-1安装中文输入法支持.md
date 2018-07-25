---
title: Ubuntu 14.04 Qt5.3.1 Fcitx 输入法支持中文
date: 2018-07-22 20:57:47
tags: [Qt,Linux]
---
## 安装fcitx for Qt5动态库

```shell
sudo apt-get install fcitx-libs-qt5
```

**注**：这一个命令执行完毕后，系统中已经具备基于Qt5的程序的汉字录入环境支持。存在一个奇怪现象是，Qt5所带的Qt Creator依然无法切换输入法，而且刚刚编译的程序，也无法录入汉字，但卸载掉Qt5开发环境后，刚刚编译的程序居然可以切换输入法，录入汉字了。这个现象说明，卸载Qt5开发环境后，同样的程序，使用系统提供的依赖库环境，录入汉字问题消失。问题出在Qt5开发环境缺少fcix for Qt5动态库上面。

## 向Qt5开发环境安装fcitx for Qt5支持

进入 _/Qt5.3.1/Tools/QtCreator/bin/plugins/platforminputcontexts_ 目录发现

官网提供的安装包仅仅有 _libibusplatforminputcontextplugin.so_，对ibus输入法的支持

```shell
cd ~/Qt5.3.1/Tools/QtCreator/bin/plugins/platforminputcontexts
cp /usr/lib/x86_64-linux-gnu/qt5/plugins/platforminputcontexts/libfcitxplatforminputcontextplugin.so .

chmod +x  libfcitxplatforminputcontextplugin.so

```

上面解决了Qt5 Creator汉字输入问题，新编译的程序运行库环境目录是_~/Qt5.3.1/5.3/gcc_64/plugins/platforminputcontexts$_

依然执行如下命令

```shell
cd ~/Qt5.3.1/5.3/gcc_64/plugins/platforminputcontexts$

cp /usr/lib/x86_64-linux-gnu/qt5/plugins/platforminputcontexts/libfcitxplatforminputcontextplugin.so .

chmod +x  libfcitxplatforminputcontextplugin.so

```



同时需要在终端中对配置文件进行修改，使用命令 —_gedit ~/.profile_ 打开配置文件，在文档末尾添加

```shell
export GTK_IM_MODULE=fcitx

export QT_IM_MODULE=fcitx

export XMODIFIERS=@im=fcitx

```

之后进行保存，重启fcitx，重启系统。

**Note**:Qt 中默认的快捷键与输入法的激活快捷键是有冲突的，需要进行设定。
