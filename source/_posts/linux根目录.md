---
title: linux根目录
date: 2017-08-27 11:50:01
tags:
    - 整理
    - linux
---
1、/bin   binary二进制
    存放系统许多可执行程序文件
    执行的相关指令，例如ls  pwd  whoami,后台的支持文件目录

2、/sbin   super binary超级的二进制
    存放系统许多可执行程序文件，许多指令支持文件，是root管理员执行
    指令的支持文件，例如init  
<!-- more -->
3、/usr   unix system resource操作系统资源文件目录
    类似windows系统的C:/Program Files目录
    是系统软件安装
```
/usr/bin    // 软件安装时形成的“普通指令文件”存放目录
/usr/sbin   // 软件安装时形成的“超级指令文件”存放目录
```

4、/dev   device设备目录
    系统把全部的硬件映射为文件存储在此目录
    例如：/dev/cdrom用于指向“光驱”
 
5、/home   家目录
    每给系统增加一个用户，都会在此目录下创建一个“同名”的文件目录作为
    该用户的家目录使用，该用户对家目录拥有绝对权限
    用户每次登陆系统首先进入其家目录。

6、/root   超级管理员root的家目录

7、/proc   内存映射目录
    可以查看系统的相关信息

8、/var  variable 可变的、易变的
    该目录存放的文件经常发生变动
    该目录用于部署程序项目
    /var/www/shop
    /var/www/book

9、/boot   系统启动目录
    /boot/initramfs-2.6.32-504.el6.i686.img是系统内核文件

10、/etc    系统主要配置文件目录
    /etc/passwd   存放系统用户信息
    /etc/group    存放系统用户组信息

11、/lib   library系统资源库目录
    

12、/selinux   secure enhanced linux安全增强型linux
    默认是开启状态，对软件安装有干扰作用


