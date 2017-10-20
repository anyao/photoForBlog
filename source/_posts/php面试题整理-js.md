---
title: php面试题整理-js
date: 2017-10-09 10:13:29
tags:
	- php面试
---
### js ###

##### 定义 #####

undefined：未使用值来声明的变量，值是undefined。

数据类型：js变量均为对象。

函数：参数无默认值。在函数外声明的变量是全局变量，所有的脚本和函数都能访问到。<!-- more -->

Window对象：window、navigator、screen、history、location。

##### eg. #####

js中为 id 是 test 的元素设置样式为 good

	document.getElementById('test').className = 'good';

### ajax ###
	
##### 工作原理 #####

XMLHttpRequest 是 ajax 的基础，其用于在后台与服务器交换数据。

XMLHttpRequest 对象请求：open(method, url, async)；

XMLHttpRequest 对象响应：responseText/responseXML；

XMLHttpRequest 相应方法：onreadystatechange reState=1,2,3,4。

通常不会考察js原生操作方法