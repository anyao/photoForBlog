---
title: php类文件的自动加载
date: 2017-10-11 22:57:13
tags:
	- php
	- 自动加载
---
#### spl\_autoload\_register() ###

在编写面向对象（OOP） 需要为每个类新建一个 PHP 文件。 这会带来一个烦恼：每个脚本的开头，都需要包含（include）一个长长的列表（每个类都有个文件）。

` spl_autoload_register()` 函数可以注册任意数量的自动加载器，当使用尚未被定义的类（class）和接口（interface）时自动去加载。通过注册自动加载器，脚本引擎在 PHP 出错失败前有了最后一个机会加载所需的类。
<!--more-->
**自动加载的主要规则**是，类名与类的定义文件一定要有关联，也就是说，我们要确保能够通过类名找到该类的类文件。
	
	```
	class CommonService{
		public static function autoloadController(){
			spl_autoload_register(function ($class_name) {
			    if (file_exists("./../model/".$class_name.".class.php")) 
					include_once './../model/'.$class_name.'.class.php';
				else{
					echo "没有找到相关的类文件<br>";
					return false;
				}
			});
		}
	}

	require_once "../model/commonService.class.php";
	CommonService::autoloadController();
	```

但是，有以下需要**注意**的几点：

- 注册一定要发生在需要某个类之前；
- 可以注册多个自动加载函数，在载入成功之前，会依次的按注册的顺序执行，直到找到为止。

### __autoload() ###

尽管 `__autoload()` 函数也能自动加载类和接口，但更建议使用 spl_autoload_register() 函数。 spl_autoload_register() 提供了一种更加灵活的方式来实现类的自动加载（同一个应用中，可以支持任意数量的加载器，比如第三方库中的）。因此，不再建议使用 __autoload() 函数，在 PHP 7.2.0.中它已经被弃用。

而且，一旦定义了其他的自动加载函数了，以前默认的__autoload函数就失效了！如果想继续生效，需要重新注册。

	spl_autoload_register('__autoload');

[类的自动加载](http://php.net/manual/zh/language.oop5.autoload.php)