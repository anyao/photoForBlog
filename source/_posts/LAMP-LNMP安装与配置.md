---
title: LAMP/LNMP安装与配置
date: 2017-10-08 16:35:47
tags:
    - lnmp
    - lamp
---
### LINUX ###

virtual box 与 centos 官网下载，安装。

virtual box 首次启动centos时，需注意：

 - **安装位置**的选择 和 **网络和主机名**中网络的开启

 - **root 密码**的设置 和**管理员用户**的创建
<!-- more -->
网络设置

- `ping www.baidu.com`，若网络不通，则 `vi /etc/sysconfig/network-scripts/ifcfg-enp0s3` 中`ONBOOT = yes`

- 设置静态 IP，`ifcfg-enp0s3` 中添加 


		IPADDR = "192.168.1.150" 
		NETMASK = "255.255.255.0"
		NM_CONTROLLED=  "no"


保存退出。


		sudo systemctl restart network.service     // 重新启动 network
		ip addr   // 查看固定 ip 是否分配成功
   
     
防火墙允许连接

    sudo firewall-cmd --zone=public --add_port=22/tcp --permanent  
    sudo systemctl restart firewalld.service // 重新启动 

virtual box的设置中的网络，选择连接方式为 **桥接网卡**，对应虚拟机右键重启。本地使用 `ssh ann@192.168.1.150` 登录。

[SecurCRT](http://ez.downcc.com/down1/securecrt64pj_downcc.zip)
        
### NGINX ###

	sudo rpm -Uvh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm
	sudo yum install -y nginx
	sudo systemctl start nginx.service
	sudo systemctl status nginx.service

[CentOS 7 yum 安装 Nginx](http://blog.csdn.net/u012486840/article/details/52610320)


### APACHE ###

安装

	sudo yum -y install httpd
	sudo rm -f /etc/httpd/conf.d/welcome.conf
	sudo vim /etc/httpd/conf/httpd.conf

配置

	ServerAdmin root@linuxprobe.org // line 86
	ServerName www.linuxprobe.org:80    // line 95
	AllowOverride All   // line 151
	DirectoryIndex index.html index.cgi index.php   // line 164
	ServerTokens Prod
	KeepAlive On

启动

	sudo systemctl start httpd
	sudo systemctl enable httpd

开启防火墙

	sudo firewall-cmd --add-service=http --permanent
	sudo firewall-cmd --reload

测试

	vim /var/www/html/index.html

	<html>
	    <body>
	        it works!!
	    </body>
	</html>


### MYSQL###

安装

	sudo rpm -Uvh https://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm
	yum repolist enabled | grep mysql
	sudo yum install mysql-community-server

启动

	sudo systemctl start mysqld.service
	
	// 查看启动状态，如果状态不是active，则查看/var/log/mysqld.log日志
	sudo systemctl status mysqld.service    
	
	// 查看初始密码
	sudo grep 'temporary password' /var/log/mysqld.log
	
	// 启动
	mysql -uroot -p

修改密码

	// 修改默认密码
	ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass4!';
	
	// 修改失败
	ERROR 1819 (HY000): Your password does not satisfy the current policy requirements
	
	// 降低密码安全等级
	set global validate_password_policy=0;


判断密码的标准就基于密码的长度了。这个由`validate_password_length`参数来决定，默认值为8。重新修改密码即可。

[A Quick Guide to Using the MySQL Yum Repository](https://dev.mysql.com/doc/mysql-yum-repo-quick-guide/en/)

卸载

源码安装cmake的各种参数会造成配置时各种麻烦，而且还没有默认my.cnf，所以暂时改用yum。而卸载没有安装好的mysql时，my.cnf不会跟着删除，也就会造成mysqld.log和初始密码找不到。

[CentOS下MySQL的彻底卸载](http://blog.csdn.net/typa01_kk/article/details/49057073)

### PHP ###

	// 先装个依赖包
	sudo rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
	
	sudo rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm
	sudo yum install php70w
	yum install php70w php70w-opcache php70w-fpm php70-mysql php70w-mbstring php70w-devel php70w-pear
	php -v


[webtatic](https://webtatic.com/projects/yum-repository/)

+ LAMP


		sudo vi /etc/php.ini
		
		date.timezone = "Asia/Shanghai" // line 878，设置时区
		
		sudo systemctl restart httpd
		
		echo "<?php phpinfo(); ?>" > /var/www/html/phpinfo.php

+ LNMP


		vim /etc/nginx/nginx.conf   // last line：看到
		
		include /etc/nginx/conf.d/*.conf;
	
	
		vim /etc/nginx/conf.d/default.conf;
		
		location ~ \.php$ {
		root /var/www/html; #指定php的根目录
		fastcgi_pass 127.0.0.1:9000;#php-fpm的默认端口是9000
		fastcgi_index index.php;
		fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
		include fastcgi_params;
		}
	
		nginx -t
		sudo systemctl restart nginx.service
		sudo systemctl start php-fpm
		
		netstat -ntpl   // 查看php-fpm有没有监听

本地访问`192.168.1.150/phpinfo.php`。

[CentOS 7 安裝 Nginx、PHP7、PHP-FPM](http://www.blogjava.net/Alpha/archive/2016/08/10/431515.html)
            
[centos7 安装LNMP（php7）之 nginx php-fpm yum安装以及配置文件修改](http://www.cnblogs.com/zhouqi666/p/6793731.html)