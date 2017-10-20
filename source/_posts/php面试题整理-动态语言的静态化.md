---
title: php面试题整理-动态语言的静态化
date: 2017-10-10 15:53:39
tags:
	- php面试
---
##### 使用模板引擎 #####

smarty 缓存机制
	
	$smarty->cache_dir = $ROOT."/cache";		// 缓存目录
	$smarty->caching = true;	// 是否开启缓存
	$smarty->cache_lifetime = "3600";	// 缓存时间

	$smarty->display(string template [, string cache_id [, string compile_id]]);

	$smarty->clear_all_cache();	// 清除所有缓存
	$smarty->clear_cache('file.html'); // 清除指定的缓存
	$smarty->clear_cache('article.html', $art_id);	// 清除同一模板下的指定缓存号的缓存 

<!-- more -->

##### 利用ob系列的函数 #####

	ob_start()：打开输出控制缓冲
	ob_get_contents()：返回输出缓冲区内容
	on_clean()：清空输出缓冲区
	ob_end_flush()：冲刷出（送出）输出缓冲区内容并关闭缓冲

	可以判断文件的inode修改时间，判断是否过期使用filetime函数

	<?php
		$cache_name = md5(__FILE__).'.html';

		$cache_lifetime = 3600;
		if( filectime(__FILE__) <= filetime($cache_name)
			file_exists($cache_name) && 
			filectime($cache_name) + $cache_lifetime > time()){
			include $cache_name;
			exit;
		}

		ob_start();
	?>
	<b> This is  My Script</b>
	<?php
		$content = ob_get_contents();
		ob_end_flush();
		$handle = fopen($cache_name, 'w');
		fwrite($handle, $content);
		fclose($handle);
	?>