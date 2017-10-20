---
title: php面试题整理-缓存机制
date: 2017-10-10 14:39:28
tags:
	- php面试
---
### HTTP ###

##### HTTP缓存分类 #####

如果请求成功，status code会有三种情况：

200 from cache：直接从本地缓存中获取响应，最快速，最省流量，因为根本没有向服务器发送请求。

304 not modified：协商缓存，浏览器在本地没有命中的情况下请求头中发送一定的校验数据到服务端，如果服务端数据没有改变，浏览器从本地缓存响应，返回304。

快速，发送的数据很少，只返回一些基本的响应头信息，数据量很小，不发送实际相应体。

200 ok
		
以上两种缓存全都失败，服务器返回完整响应。没有用缓存，相对最慢。

<!-- more -->

##### 优先级 #####

Pragma > Cache-Control > Expires

##### 缓存策略 #####
		
适合缓存的内容

- 不变的图像，eg. logo，图标等
- js、css静态文件
- 可下载文件，mp3等

适合协商缓存

- HTML文件
- 经常替换的图片
- 经常修改的js、css文件（js、css文件的加载可以加入文件的签名来拒绝缓存 index.css?signature/index.signature.css）

不适合缓存的内容

- 用户隐私等敏感数据
			
- 经常改变的api数据接口

##### 实现 #####

	$since = $_SERVER['HTTP_IF_MODIFIED_SINCE'];
	$lifetime = 3600;
	if(strotime($since) +$lifetime > time()){
		header('HTTP/1.1 304 Not Modified');
		exit();
	}
	header('Last-Modified:'.gmdate('D, d M Y H:i:s', time()), ' GMT');

### NGINX ###

##### 本地缓存配置 #####

add_header指令： 添加状态码为 2XX 和 3XX 的响应头信息，可以设置 Pragma/Expires/Cache-Control 可以继承。

	add_header name value [always];
	
	eg. add_header cache-control max-age=3600;

expires指令：通知浏览器过期时长。time为负值时，表示Cache-Control: no-cache；为正值或者0时，就表示Cache-Control:max-age=指定时间。

	expires time;
				
Etag指令：指定签名
	
	etag on|off

![](http://wx1.sinaimg.cn/mw690/82e79a0fly1fkeo6c7srvj20jy0m3gmt.jpg)

### 前端代码和资源的压缩 ###

nginx配置

	gzip on|off; # 是否开启
	gzip_buffers 32 4K| 16 8K # 缓冲（在内存中缓存几块？每块多大）
	gzip_comp_level [1-9] # 推荐6 压缩级别（级别越高，压的越小，越浪费cpu计算资源）
	gzip_tyes text/plain 

### 数据库 ###

##### 缓存原因 #####

用户请求 --> 数据查询 --> 连接数据库服务器并查询数据 --> 将数据缓存起来（HTML、内存、JSON、序列化数据） --> 显示给客户端

用户再次请求或者新用户访问 --> 数据查询 --> 直接从缓存中获取数据 --> 显示个客户端

##### memcache #####

工作原理

一个高性能的分布式的内存对象缓存系统，通过内存里维护一个统一的巨大的hash表，它能够用来存储各种格式的数据，包括图像、视频、文件以及数据库检索的结果等。简单的说就是将数据调用到内存，然后从内存中读取，从而大大提高读取速度。

通用缓存机制
			
用查询的方法名 + 参数作为查询时的key-value对中的key

##### redis #####

与memcache的区别
			
性能相差不大
			
- redis在2.0之后增加VM特性，突破物理内存的限制；memcache可以修改最大可用内存，采用LRU算法
			
- redis 依赖客户端来实现分布式读写
			
- memcache本身没有数据冗余机制
			
- redis支持（快照、AOF），依赖快照进行持久化，aof增强可靠性的同时，对性能有所影响；memcache不支持持久化
			
- memcache在并发的场景下，用cas保持一致性，redis事物支持比较弱，只能保证事物中的每个操作连续执行
			
- redis支持多种类的数据类型

- redis用于数据量较小的高性能操作和运算上
			
- memcache用于动态系统中减少数据库负载，提升性能；适合做缓存。

