---
title: php面试题整理-CDN加速
date: 2017-10-10 14:49:01
tags:
	- php面试
---
Content Delivery Network 内容分发网络
		
在网络各处放置节点服务器所构成的在现有的互联网基础之上的一层智能虚拟网络。CDN系统能够实时地根据网络流量和各节点的连接、负载状况以及到用户的距离和响应时间等综合信息将用户的请求重新导向离用户最近的服务节点上。
<!-- more -->
### 优势 ###

- 本地Cache加速，提高了企业站点（尤其含有大量图片和静态网页站点）的访问速度
	
- 跨运营商的网络加速，保证不同网络的用户都得到良好的访问质量

- 远程访问用户根据DNS负载均衡技术智能自动选择Cache服务器
	
- 自动生成服务器的远程Mirror（镜像）cache服务器，远程用户访问时从cache服务器上读取数据，减少远程访问的带宽、分担网络流量、减轻原站点web服务器负载等功能
	
- 广泛分布的CDN节点加上节点之间的智能冗余机制，可以有效地预防黑客入侵

### 工作原理 ###
		
传统访问：用户在浏览器输入域名发起请求 --> 解析域名获取服务器IP地址 --> 根据IP地址找到对应的服务器 --> 服务器相应并返回数据
		
使用CDN访问：用户发起请求 --> 智能DNS的解析（根据IP判断地理位置、接入网类型、选择路由最短和负载最轻的服务器） --> 取得缓存服务器IP --> 把内容返回给用户（如果缓存中有） --> 向源站发起请求 --> 将结果返回给用户 --> 将结果存入缓存服务器

### 实现 ###
		
- BAT等提供CDN服务
		
- 可用LVS做4层负载均衡
		
- 可用Nginx、Varnish、Squid、Apache TrafficServer做7层负载均衡和cache

### 反向代理实现 ###

squid、nginx