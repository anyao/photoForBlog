---
title: redis入门指南
date: 2017-09-01 17:37:26
tags:
    - 读书笔记
    - redis
---
- DEL 命令的参数不支持通配符，但可以结合 linux 的 pipe 和 xargs 命自己实现删除所有符合规则的键。
<!-- more -->
eg. 
redis-cli KEYS "user:*" | xargs redis-cli DEL
redis-cli DEL 'redis-cli KEYS "user:*"' //性能更好

- redis 对于键的命名并没有强制的要求，但比较好的实践是用 “对象类型：对象ID：对象属性” 来命名一个键。
eg. user:1:friends

- HSETNX 与 HSET 命令类似，区别在于如果字段已经存在，HSETNX 命令将不执行任何操作。
HSETNX key field value
HSETNX 原子地实现了 HEXISTS 和 HSET 两个命令以避免竞态条件。

- KEYS 需要遍历数据库中的所有键。

- snap shotting 默认开启，一次性把 redis 中全部的数据保存一份存储在硬盘中，如果数据非常多（10G-20G）就不适合频繁持久化操作。

- 优化 aof ，./redis-cli bgrewriteaof。

- LTRIM 常和 LPUSH 一起使用来限制列表中的数量，比如记录日志时我们希望只保留最近的 100 条日志，则每次加入新元素时调用一次 LTRIM 即可。
LPUSH logs $newlog
LTRIM logs 0 99

- 如果使用 WATCH 检测了一个拥有生命时间的键，该键时间到期自动删除并不会被 WATCH 认为该键被改变。、

- 在对有序集合类型排序时会忽略元素的分数，只针对元素自身的值进行排序。eg：
ZADD myzset 50 2 40 3 20 1 60 5
SORT myzset
输出结果：
1) "1"
2) "2"
3) "3"
4) "5"

- SORT 的时间复杂度是 `O(n+mlogm)`，其中 n 表示要排序的的列表（集合或有序集合）中的元素个数，m 表示要返回的元素个数。当 n 较大时 SORT 的性能相对较低，并且 redis 在排序前会建立一个长度为 n 的容器来存储待排序的元素，虽然是一个临时的过程，但如果同时进行较多的大数据量排序操作则会严重影响性能。
所以在开发中使用 SORT 时需要注意以下几点。
    * 尽可能减小待排序键中元素的数量（使 n 尽可能小）。
    * 使用 LIMIT 只获取需要元素的数量 （使 m 尽可能小）。
    * 如果要排序的数据数量较大，尽可能使用 STORE 将结果缓存。

- `BRPOP` 和 `RPOP` 相似，唯一的区别是当列表中没有元素是 `BRPOP` 会一直阻塞住连接，知道有新元素加入。

- 当通过配置文件参数 maxmemory 设置了 redis 可用的最大空间大小时，redis不会使用共享对象，因为对于每一个键值都需要使用一个 redisObject 来记录其 LRU 信息。 

- 手动发送 `SAVE` 或 `BGSAVE` 命令让 redis 执行快照，两个命令的区别在于，前者是由主进程进行快照操作，会阻塞其他请求，后者会通过 fork 子进程进行快照操作。
