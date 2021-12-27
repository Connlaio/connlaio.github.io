---
title: Ubuntu下配置Samba实现文件夹共享
comments: true
toc: true
categories:
  - Linux
  - 运维
date: 2021-12-27 15:26:52
tags: 日常软件
---

**转载文章，[原链接](https://www.cnblogs.com/adong7639/p/7832046.html)**

## Samba安装

```shell
sudo apt-get install samba
 sudo apt-get install cifs-utils
```

## 创建共享目录

```shell
    mkdir /home/linux/share
    sudo chmod 777 /home/linux/share
```



## 创建配置文件

```shell
  #save the previous config file.
  sudo cp /etc/samba/smb.conf /etc/samba/smb.conf.bak
#change the config
    sudo vim /etc/samba/smb.conf

```

 	配置分两种模式

### Share模式

**所有的用户都可以直接访问不需要用户名和密码，无需samba用户就可以访问服务器 **

```shell
[Global]
security = share 

#在smb.conf最后添加

[share]
      path = /home/linux/share
      available = yes
      browseable = yes
      public = yes
      writable = yes
```

### User模式

**user级别的samba则需以samba用户和密码才能访问**

```shell
[Global]
security = user
[share]
      path = /home/linux/share
      available = yes
      browseable = yes
      public = no
      writable = yes
```

## 创建Samba账户

```shell
 sudo touch /etc/samba/smbpasswd
  sudo smbpasswd -a linux
```



## 重启Samba服务

```shell
sudo /etc/init.d/smbd restart
```



## 创建盘符

## 附件问题

**使用 samba 以读写方式共享的文件夹，为什么从其他计算机访问时所创建的文件属于 nobody / nogroup？如何更改默认属主和组？**

```shell
sudo vim /etc/samba/smb.conf

#修改配置文件如下:
#在 [global] 放入以下内容
force user = 帐号
force group = 群组
create mask = 0664
directory mask = 0775


#存档，重启smbd
sudo service smbd restart

```

## 部分相关指令

```shell
 #查看用户
sudo pdbedit -L

# 修改密码
sudo smbpasswd user

# 删除用户
sudo smbpasswd -x user
```

