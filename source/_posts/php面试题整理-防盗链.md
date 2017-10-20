---
title: php面试题整理-防盗链
date: 2017-10-10 11:36:19
tags:
	- php面试
---
### 工作原理 ###

通过 `referer` 或者签名，网站可以检测目标网页访问的来源网页，如果是资源文件，则可以跟踪到显示它的网页地址；一旦检测到来源不是本站即进行阻止或者返回指定页面。
<!--more-->
### 方法 ###

##### referer #####

- nginx模块 `ngx_http_referer_module` 用于阻挡来源非法的域名请求

- nginx命令 `valid_referers`，全局变量`$invalid_referer`


		valid_referers none | blocked | server_names | string...

		location ~.*\.(gif|jpg|png|flv|swf|rar|zip)$
		// 或者针对目录的防盗链
		// location /images/
		{
			valid_referers none blocked test.com *.test.com;
			if($invalid_referer){
				#return 403;
				rewrite ^/http://403.jpg;
			}
		}

##### 签名 #####

伪造referer的情况，可以使用加密签名解决。使用第三方模块HttpAccessKeyModule实现Nginx防盗链。

	accesskey on|off 模块开关
	accesskey_hashmethod md5 | sha-1 签名加密方式
	accesskey_arg GET 参数名称
	accesskey_signature 加密规则

	location ~.*\.(gif|jpg|png|flv|swf|rar|zip)$
	{
		accesskey_on;
		accesskey_hashmethod md5;
		accesskey_arg "key";
		accesskey_signature "mypass$remote_addr";
	}

	$sign = md5('key');
	echo '<img src="./logo.jpg?sign='.$sign.'">';