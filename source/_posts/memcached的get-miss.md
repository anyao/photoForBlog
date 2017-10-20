---
title: memcached的get_miss
date: 2017-09-21 22:20:37
tags:
    - memcached
    - LRU
    - 懒惰模式
---
- **缓存过期**
memcached 在处理过期的缓存项时，采用**懒惰模式**处理方法。
缓存项过期，不会立即删除，直到对该缓存项执行了`get`操作，才会删除过期的缓存项。
<!-- more -->

- **缓存空间已满**
memcached 再插入新数据时，如果空间不足（相同 chunk，一个 slab 里）采用删除旧缓存项的策略。采用删除最少最近使用（使用频率低）的缓存项（**LRU策略** Least Recently Used）

P.S.  [http://blog.csdn.net](http://blog.csdn.net/qianshangding0708/article/details/47980697)