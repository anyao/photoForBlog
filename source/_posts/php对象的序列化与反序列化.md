---
title: php对象的序列化与反序列化
date: 2017-10-12 08:07:38
tags:
	- php
	- 序列化
---
对象的序列化也叫作对象的串行化。序列化主要解决数据的持久化存储的问题。对象也是一种数据类型，既然是数据，就也存在被持久化存储的问题。
<!--more-->
### 数据保存在文件中 ###

文件写入数据：`file_put_contents(文件地址,数据内容)`

从文件读取数据：`file_get_contents(文件地址)`

由于文件内部最终保存的只能是字符串类型，所以，任何数据在写入文件之前都要进行类型的自动转换，也就是说，只要当初保存的数据不是字符串类型，读取的时候都有可能出现问题（true 和 1）。

所以，应该在数据转换为字符串的时候，应该同时在文件内记录原始数据的值和类型等其他信息，目的就是在读取数据的时候，能够通过这些额外信息得到原始数据，上面的两个过程，就叫作数据的序列化与反序列化。

### 序列化与反序列化 ###

`serialize()`：序列化，将原始数据转换为可以用户保存和传输的字符串数据

`unserialize()`：反序列化，将被序列化的数据转换为原始数据

### PHP\_Incomplete\_Class ###

有时反序列化之后类名变成了__PHP_Incomplete_Class，是一个系统预定义的类。

因为在反序列化一个对象的时候，同样的需要先找到该对象所属的类，如果找不到，就被反序列化成了系统预定义的类__PHP_Incomplete_Class。

解决方案：在读取对象的脚本中引入相关的类文件。

### 资源型数据的序列化 ###

一般来说，对象的所有的属性都要被序列化，但是，在某些类中，一定存在不需要被序列化的数据，比如资源型数据（序列化为0毫无意义），应该将不需要被序列化的属性给人为的去掉！这个过程是由`__sleep()`方法来完成的。

##### __sleep() #####

触发时机：在序列化一个对象的时候自动由系统调用。该方法用于规定哪些数据应该被序列化，实现的方式为：返回一个索引数组，数组内的元素为需要被序列化的属性名的集合。

	public function __sleep(){
		return ['host', 'port', 'user', 'pass', 'charset', 'dbname']
	}

这样，序列化之后对象的资源型属性就会为空。

##### __wakeup() #####

如果想反序列化之后，自动完成相关初始化操作，则可以使用 __wakeup()。

	public function __wakeup(){
		$this->connect();
	}








