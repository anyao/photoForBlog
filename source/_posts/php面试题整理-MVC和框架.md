---
title: php面试题整理-MVC和框架
date: 2017-10-09 15:54:26
tags:
	- php面试
---
### MVC ###

单一入口：用一个处理程序文件处理所有的HTTP请求，根据请求时的参数的不用不同区分不同模块和操作的请求。

+ 优点

 - 统一的安全性检查

 - 集中处理程序
		
+ 缺点

 - url不美观（url重写）

 - 处理效率会稍低
 <!-- more -->
模板引擎：php是一种html内嵌式的在服务端执行的脚本语言，但有很多可以使php代码和html代码分开的模板引擎。实质是庞大的完善的正则表达式替换库。eg. 
	
	smarty $smarty->assign('name', 'annyao');



### 框架 ###

准备最印象深刻的功能和特性，进行框架质检的对比，展现对php框架的理解，侧面说明开发经验。

- yaf：C语言为底层，性能上要比其他快一个数量级。执行效率高，轻量级框架，可扩展性强；高版本兼容性差，底层代码可读性差，需要安装拓展，功能单一，开发需要大量插件。

- yii2：结构简单优雅、实用功能丰富、拓展性强、性能高；学习成本高，相比yaf，量级较重。


#### eg. #### 

yii2如何实现数据的自动验证？
		
model里编写rules验证规则，在进行数据接收之后调用相应方法进行验证