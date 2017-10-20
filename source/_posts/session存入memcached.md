---
title: session存入memcached
date: 2017-09-21 22:52:19
tags:
    - memcached
    - session
    - php
---
- **方法一**
使用 memcached 拓展，提供的 session 处理器 （session.save_handler）：memcache。<!---more-->
通过配置存储位置配置项（session.save_path），设置 memcached 服务器信息。
```
<?php
# memcache 拓展定义好的memcache，session存储处理器
ini_set('session.save_handler', 'memcache');

# 所使用的 memcached 服务器信息
ini_set('session.save_path', 'tcp://127.0.0.1:11211; tcp://host2:port2; tcp://host3:port3;');
?>
```
需要用于 `session_start();` 之前。


- **方法二**
更改 php.ini 配置。

```
;session.save_handler = files
session.save_handler = memcache

;session.save_path = "/tmp"
session.save_path = "tcp://127.0.0.1:11211"
```


P.S. 在 memcache 中 session 以  session_id为键名保存。
