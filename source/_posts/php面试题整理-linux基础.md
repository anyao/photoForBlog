---
title: php面试题整理-linux基础
date: 2017-10-09 11:07:42
tags:
	- php面试

	- shell
---
### 常用命令 ###

系统安全 

	sudo su chmod setfacl...

进程管理 

	w top ps kill pkill pstree killall...

用户管理 

    id usermod useradd groupadd userdel...
<!--more-->
文件系统 

    mount umount fsck df du...

关机重启 

    shutdown reboot...

网络应用 

    curl telnet mail elinks...

网络测试 

    ping netstat host...

网络配置 

    hostname ifconfig...

常用工具 

    ssh screen clear who date...

软件包管理 

    yum rpm apt-get...

文件查找和比较 

    locate find...

文件内容的查看 

    head tail less more...

文件处理 

    touch unlink rename ln cat...

目录操作 

    cd mv rm pwd tree cp ls...

### 定时任务 ###

	crontab -e 
	分 时 日 月 周 command
	at 2:00 tomorrow
	at> /home/Ann/do_job
	at> Ctrl+D

### shell基础 ###

##### 脚本执行方式 #####

- 赋予权限，直接执行，eg. 


		chmod + x test.sh; ./test.sh


- 调用解释器使用脚本执行，eg. 


		bash、csh、ash、bsh、ksh...


- source eg. 


		source test.sh


##### 编写基础 #####

开头用 `#!` 指定脚本解释器，eg. 

	$ #!/bin/sh

编写具体功能

### eg. ###

实现每天0点钟重新启动服务器

	crontab -e

	0 0 * * * reboot