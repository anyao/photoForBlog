---
title: js关于$.each遍历
date: 2017-10-11 14:49:44
tags:
	- js
	- jquery
	- 遍历
---
##### each #####

each函数根据参数的类型实现的效果不完全一致：

1、遍历对象(有附加参数)

	$.each(Object, function(p1, p2) {
		this; //这里的this指向每次遍历中Object的当前属性值
		p1; p2; //访问附加参数
	}, ['参数1', '参数2']);
<!--more-->
2、遍历数组(有附件参数)

	$.each(Array, function(p1, p2){
		this; //这里的this指向每次遍历中Array的当前元素
		p1; p2; //访问附加参数
	}, ['参数1', '参数2']);

3、遍历对象(没有附加参数)

	$.each(Object, function(name, value) {
		this; //this指向当前属性的值
		name; //name表示Object当前属性的名称
		value; //value表示Object当前属性的值
	});

4、遍历数组(没有附加参数)

	$.each(Array, function(i, value) {
		this; //this指向当前元素
		i; //i表示Array当前下标
		value; //value表示Array当前元素
	});

##### for #####

for 遍历 json 对象

	var myJson = {"name":"ann", "password":"521"};

	//遍历json对象的每个key/value对,p为key
	for(var p in myJson){
	
	   alert(p + " " + myJson[p]);
	
	}
