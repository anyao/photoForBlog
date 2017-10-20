---
title: php面试题整理-减少HTTP请求
date: 2017-10-10 13:34:29
tags:
	- php面试
---
##### 性能黄金法则 #####
		
只有10%-20%的最终用户响应时间花在接收请求的HTML文档上，剩下的80%-90%的时间花在HTML文档所引用的所有组件（图片、script、css、flash等等）进行的HTTP请求上
<!--more-->
##### 减少HTTP请求原因 #####

域名解析--tcp连接--发送请求--等待--下载资源--解析时间

查找DNS缓存也需要时间，多个缓存就要查找多次，有可能缓存会被清除

HTTP1.1协议规定请求只能串行发送，也就是说一百个请求必须依次逐个发送，前面的一个请求完成才能开始下个请求

	
##### 减少HTTP请求方式 #####

1、图片地图
	
图片地图允许你在一个图片上关联多个URL。目标URL的选择取决于用户单击了图片的那个位置

	<img usemap="#map1" src="/test.gif">
	<map name="map1">
		<area shape="rect" coords="0,0,31,31" href="javascript:alert('button1')" title="button1">
		<area shape="rect" coords="36,0,66,31" href="javascript:alert('button2')" title="button2">
	</map>

2、css sprites

css精灵，通过使用合并图片，通过指定的css的backgroud-image和background-position来显示元素

	background-position:x y;

	#navbar span{
		width: 31px;
		height: 31px
		background-image: url(/test.gif);
	}

	.button1{
		background-positon: 0 0; // 向右下移动都为负
	}

3、合并脚本和样式表

独立的一个js要比多个js文件组成的页面要快38%

4、图片使用base64编码减少页面请求数
			
将图片直接嵌入到页面中，而不是从外部载入

	<img src="data:image/gif;base64,/9j/4AAQkZJ..">
	
	php自带函数

##### 性能影响 #####

图片地图与css精灵的响应时间基本相同，但比使用各自独立图片的方式要快50%以上
