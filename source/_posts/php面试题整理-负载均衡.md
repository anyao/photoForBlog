---
title: php面试题整理-负载均衡
date: 2017-10-10 21:50:29
tags:
	- php面试
---
#### 七层 ####

基于URL等应用层信息的负载均衡

#### nginx的proxy ####
 
- 能够自动剔除工作不正常的后端服务器
	
- 上传文件使用异步模式
	
- 支持多种分配策略，可以分配权重，分配方式灵活
<!-- more -->
##### 内置策略 #####

1、加权轮询

首先将请求都分给高权重的机器，知道该机器的权值降到了比其他机器低，才开始将请求分给下一个高权重的机器；

当所有后端机器都down掉时，nginx会立即将所有机器的标志位清成初始状态，以避免造成所有的机器都处在timeout的状态。

2、IP Hash

流程和轮询类似，只是其中的算法和具体的策略有些变化。是一种变相的轮询算法。

##### 扩展策略 ######

1、fair策略

根据后端服务器的响应时间判断负载情况，从中选出负载最轻的机器进行分流。

2、通用hash、一致性hash策略

- 通用hash比较简单，可以以nginx内置的变量为key进行hash
			
- 一致性hash采用nginx内置的一致性hash环，支持mamcache

##### nginx配置 #####

	http{
		upstream cluster{
			server srv1;
			server srv2;
			server srv3;
		}

		server{
			listen 80;
			location /{
				proxy_pass http://cluster;
			}
		}
	}

### 四层 ###

- 通过报文中的目标地址和端口，再加上负载均衡设备设置的服务器选择方式，决定最终选择的内部服务器
		
- LVS实现服务器集群负载均衡有三种，NAT、DR和TUN