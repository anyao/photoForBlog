---
title: php面试题整理-会话控制技术
date: 2017-10-08 15:21:04
tags:
    - php面试
---
为什么使用：HTTP协议为无状态协议（没有内建机制维护两个事物之间的状态），所以同一个用户两次访问相同页面时，HTTP请求不会认为它们来自同一个用户，会把它们当做是两次请求的独立，也就没有办法在不同页面之间保持跟踪。允许服务器跟踪同一个客户端做出的连续请求。

<!-- more -->

### session的垃圾回收机制 ###

在PHP中，没有任何变量指向这个对象时，这个对象就成为垃圾。PHP会将其在内存中销毁；这是PHP的GC垃圾处理机制，防止内存溢出。

GC的工作就是扫描所有的Session信息，用当前时间减去session最后修改的时间，同 `session.gc_maxlifetime` 参数进行比较，如果生存时间超过 `gc_maxlifetime` (默认24分钟),就将该session删除。 

当一个有效的请求发生时，PHP 会根据全局变量  `session.gc_probability` 和 `session.gc_divisor` 的值，来决定是否启用一个GC。

	session.gc_maxlifetime = 30
	session.gc_divisor = 1000
	session.gc_probability = 1


表示每一千个用户调用session_start()的时候，就百分百的会执行一次垃圾回收机制，将磁盘上没用的session文件删除。

### 禁用cookie时 ###
	
	<a href="1.php?<?= session_name().'='.session_id();?>">
	<a href="1.php?<?= SID?>">


### session存储 ###

![](http://img.blog.csdn.net/20171008162601796)

#### 方法一 ####
    
使用 memcached 拓展，提供的 session 处理器 （session.save_handler）：memcache。
通过配置存储位置配置项（session.save_path），设置 memcached 服务器信息。

    <?php
    # memcache 拓展定义好的memcache，session存储处理器
    ini_set('session.save_handler', 'memcache');
    
    # 所使用的 memcached 服务器信息
    ini_set('session.save_path', 'tcp://127.0.0.1:11211; tcp://host2:port2; tcp://host3:port3;');
    ?>

需要用于 `session_start();` 之前。
    
#### 方法二 ####

更改 php.ini 配置。
        
	;session.save_handler = files
	session.save_handler = memcache
	
	;session.save_path = "/tmp"
	session.save_path = "tcp://127.0.0.1:11211"
        
P.S. 在 memcache 中 session 以  session_id为键名保存。
