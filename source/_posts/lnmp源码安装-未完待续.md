---
title: lnmp源码安装-未完待续
date: 2017-10-06 22:11:26
tags:
---
- **LINUX**
<!-- more -->

1. virtual box 与 centos 官网下载，安装。



2. virtual box 首次启动centos时，需注意：

        +  **安装位置**的选择 和 **网络和主机名**中网络的开启

        + **root 密码**的设置 和**管理员用户**的创建
        
3. 网络设置

        +  `ping www.baidu.com`，若网络不通，则 `vi /etc/sysconfig/network-scripts/ifcfg-enp0s3` 中`ONBOOT = yes`
        
        + 设置静态 IP，`ifcfg-enp0s3` 中 `BOOTPROTO = "static"` ，添加 
        
                        ```
                        IPADDR = "192.168.1.150"
                        NETMASK = "255.255.255.0"
                        NM_CONTROLLED=  "no"
                        ```
                
                        保存退出。
                        
                        ```
                        sudo systemctl restart network.service     // 重新启动 network
                        ip addr   // 查看固定 ip 是否分配成功
                        ```
                        
        + 防火墙允许连接
        
                        ```
                        sudo firewall-cmd --zone=public --add-port=22/tcp --permanent  
                        sudo systemctl restart firewalld.service // 重新启动 
                        ``` 
          virtual box的设置中的网络，选择连接方式为 **桥接网卡**，对应虚拟机右键重启。本地使用 `ssh ann@192.168.1.150` 登录。

- **PHP**

1. 先安装个编辑器：

                ```
                sudo yum install vim
                ```
                
                P.S. 首次 `sudo` 会报错误 `is not in the sudoers file` 的错误。解决办法：

                ```
                visudo  // root 权限下运行
                
                // 在打开的配置文件中，找到root ALL=(ALL) ALL，在下面添加一行，其中xxx是你要加入的用户名称
                xxx ALL=(ALL) ALL 
                ```

2.  安装：

                ```
                sudo yum install wget
                sudo wget http://cn2.php.net/get/php-7.1.10.tar.gz/from/this/mirror
                tar -zxvf mirror
                ```
                
                P.S. 单纯安装php是不能和nginx一起工作的，需要安装php-fpm。
                
                ```
                sudo yum install gcc gcc++ libxml2-devel  // 安装依赖库
                cd php-7.1.10/
                ./configure --prefix=/usr/local/php --enable-fpm
                make
                sudo make install
                
                // make a test
                cd 
                vim test.php
                
                // test.php 
                <?php
                        phpinfo();
                        
                /usr/local/php/bin/php test.php // 返回信息则表示安装成功
                ```
- **MYSQL**

1. 官网下载 MYSQL Community Server 里的 Source Code，选择 Generic Linux 
        
        ```
        wget https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.19.tar.gz
        tar -zxvf mysql-5.7.19.tar.gz
        sudo yum install cmake gcc-c++ ncurses-devel perl-Data-Dumper boost boost-doc boost-devel
        cd mysql-5.7.19
        cmake ↓
        sudo make
        sudo make install
        ```
        
        接下来说用 cmake 来安装mysql
        
        ```
        cmake
        -DMYSQL_DATADIR=/usr/local/mysql/data \
        -DSYSCONFDIR=/etc \
        -DMYSQL_USER=mysql \
        -DWITH_MYISAM_STORAGE_ENGINE=1 \
        -DWITH_INNOBASE_STORAGE_ENGINE=1 \
        -DWITH_ARCHIVE_STORAGE_ENGINE=1 \
        -DWITH_READLINE=1 \
        -DMYSQL_UNIX_ADDR=/usr/local/mysql/data/mysql.sock \
        -DMYSQL_TCP_PORT=3306 \
        -DENABLED_LOCAL_INFILE=1 \
        -DENABLED_DOWNLOADS=1 \
        -DWITH_PARTITION_STORAGE_ENGINE=1 \
        -DEXTRA_CHARSETS=all \
        -DDEFAULT_CHARSET=utf8 \
        -DDEFAULT_COLLATION=utf8_general_ci \
        -DWITH_DEBUG=0 \
        -DMYSQL_MAINTAINER_MODE=1 \
        -DWITH_SSL:STRING=bundled \
        -DWITH_ZLIB:STRING=bundled \
        -DDOWNLOAD_BOOST=1 \
        -DWITH_BOOST=/usr/share/doc/boost/
        ```


