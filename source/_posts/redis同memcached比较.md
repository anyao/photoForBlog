---
title: redis同memcached比较
date: 2017-08-30 21:18:58
tags:
---
1、redis 不仅仅支持简单的 key/value 类型的数据，同时还提供 list，set，zset，hash 等结构的存储。
2、redis 支持 master-slave 模式应用。
3、redis 支持数据的持久化，可以将内存中的数据保存到磁盘中，重启的时候可以再次加载进行使用。
4、redis 单个 value 的最大限制是 1GB，memcached只能保存 1MB 的数据。