---
title: php面试题整理-文件及目录处理
date: 2017-10-08 10:49:32
tags:
    - php面试
---
+ **文件操作函数及几种打开模式**

	1. fopen()
		
		>r 打开只读文件，该文件必须存在。
		> 
		>r+ 打开可读写的文件，该文件必须存在。 
		> 
		>w 打开只写文件，若文件存在则文件长度清为0，即该文件内容会消失。若文件不存在则建立该文件。
		> 
		> w+ 打开可读写文件，若文件存在则文件长度清为零，即该文件内容会消失。若文件不存在则建立该文件。
		> 
		> a 以附加的方式打开只写文件。若文件不存在，则会建立该文件，如果文件存在，写入的数据会被加到文件尾，即文件原先的内容会被保留。
		> 
		>a+ 以附加方式打开可读写的文件。若文件不存在，则会建立该文件，如果文件存在，写入的数据会被加到文件尾后，即文件原先的内容会被保留。 
		>
		> x 创建并以写入方式打开，将文件指针指向文件头。如果文件已存在，则 fopen() 调用失败并返回 FALSE，并生成一条 E_WARNING 级别的错误信息。如果文件不存在则尝试创建之。
		> 
		>x+ 创建并以读写方式打开，其他的行为和 'x' 一样。
		<!-- more --> 	
		P.S. 上述的形态字符串都可以再加一个b字符，如rb、w+b或ab＋等组合，加入b 字符用来告诉函数库打开的文件为二进制文件，而非纯文字文件。为移植性考虑，强烈建议在用 fopen() 打开文件时总是使用 'b' 标记。
	
	2. 其它
	
		写入：fwrite()、fputs()
		
		读取：fread()、fgets() 获取一行、fgetc() 获取一个字符、file() 内容读取到数组中取、readfile() 读取并且输出到缓冲区
		
		关闭：fclose()
	
		不需要fopen打开的函数：file_get_contents()、file_put_contents()

+ **目录操作函数**

	1. 名称相关 

			```
			// basename(path,suffix) 返回路径中的文件名部分
			$path = "/testweb/home.php";
			echo basename($path);   // home.php
			echo basename($path,".php");    // home
	
	
			//dirname(path) 返回路径中的目录部分
			echo dirname("c:/testweb/home.php");    // c:/testweb
	
	
			// pathinfo(path,options) 返回文件路径的信息
			print_r(pathinfo("/testweb/test.txt"));
			Array(
			    [dirname] => /testweb
			    [basename] => test.txt
			    [extension] => txt
			)
			```

	2. 目录读取 opendir()、readdir()、closedir()、rewinddir()
	
	3. 目录删除 rmdir()（目录下必须为空）
	
	4. 目录创建 mkdir()
	
	5. 目录大小 disk_free_space()、disk_total_space() 磁盘
	
	6. 文件大小 fielsize()
	
	7. 文件复制 copy()
	
	8. 文件删除 unlink()
	
	9. 文件类型 filetype() （file or dir）
	
	10. 文件或者目录重命名 rename()
	
	11. 文件截取 ftruncate()
	
	12. 文件属性 file_exists()...

- **eg.** 

		```
		// 文件前加hello world
		$file = './hello.txt';
		$handle = fopen($file, 'r');
		$content = fread($handle, $filesize($file));
		$content = 'Hello World'.$content;
		fclose($handle);
		$handle = fopen($file, 'w');
		fwrite($handle, $content);
		$fclose($handle);
	
		// 对目录进行遍历
		function loopDir($dir){
		   $handle = opendir($dir);
		   while(false !== ($file = readdir($handle))){
		       if ($file != '.' && $file != '..'){
		           echo $file."\n";
		           if (file($dir.'/'.$file) == 'dir'){
		               loopdir($dir.'/'.$file);
		           }
		       }
		   }
		}
		```
