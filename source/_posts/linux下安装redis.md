---
title: linux下安装redis
date: 2017-08-27 20:13:10
tags:
    - redis
    - linux
---
1、准备源码包
```
cp redis.tar.gz /usr/local/src/
```

2、解压
```
root@AnnS:/usr/local/src# tar -zxvf redis.tar.gz
```
<!-- more -->
3、 直接make
```
root@AnnS:/usr/local/src/redis# make  -- 不需要使用 ./configure
```

4、把 redis 安装到指定目录
```
root@AnnS:/usr/local/src/redis# make PREFIX=/usr/local/redis install
```

5、复制 redis 的配置文件
```
make /etc/redis/
cp redis.conf /etc/redis/
```

6、启动 redis
```
root@AnnS:/usr/local/redis/bin# ./redis-server
```

7、后台运行
```
root@AnnS:/usr/local/redis/bin# gedit /etc/redis/redis.conf -- 修改daemonize no 为 yes
root@AnnS:/usr/local/redis/bin# ./redis-server /etc/redis/redis.conf
```

8、查看是否正常启动 ps/netstat
```
ps axu | grep redis-server
netstat -tunple | grep 6379
```

9、关闭后台
```
pkill -9 redis-server
```

10、连接
```
root@AnnS:/usr/local/redis/bin# ./redis-cli -h 192.168.1.108 -p 6379
```