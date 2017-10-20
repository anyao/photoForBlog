---
title: php面试题整理-正则表达式
date: 2017-10-08 10:29:29
tags:
    - php面试
---
作用：分割、查找、匹配、替换字符串

应该掌握的函数：preg_mathch()、preg_match_all()、preg_replace()、preg_split()
<!-- more -->
+ 后向引用

	```
	$str = '<b>abc</b>';
	$pattern = '/<b>(.*)<\/b>/';
	preg_replace($pattern, '\\1', $str);
	```

+ 贪婪模式

	```
	$str = '<b>abc</b><b>bcd</b>';
	
	$pattern = '/<b>.*?<\/b>/'; // or
	$pattern = '/<b>.*</b>U';
	
	preg_replace_all($pattern, '\\1', $str);
	```

+ 中文匹配

	utf-8汉字编码范围0x4e00-0x9fa5，在ANSI（gb2312）下，0xb0，0xa1-0xfe
	
	utf-8要使用u模式修正符使模式字符串被当成utf-8，在ANSI（gb2312）环境下，要使用chr将Ascii码转换为字符

	```
	$pattern = '/[\x{4e00}-\x{9fa5}+/u';    // utf-8
	$pattern = '/['.chr(0xb0).'-'.chr(0xf7).']['.chr(0xa1).'-'.chr(0xfe).']/';  // gb2312
	
	// eg.
	$pattern = '/^139\d{8}$/';
	$pattern = '/<img.*?src="(.*?)".*?\/?>/i';
	```