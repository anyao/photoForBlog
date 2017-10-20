---
title: php面试题整理-并发处理
date: 2017-10-10 16:44:24
tags:
	- php面试
---
##### 线程与进程的区别 #####

1、线程是进程内的一个执行单元，进程内至少有一个线程，它们共享进程的地址空间，而进程有自己独立的地址空间
		
2、进程是资源分配和拥有的单位，同一个进程内的线程共享进程的资源
		
3、线程是处理器调度的基本单元，但进程不是
		
4、两者均可并发执行
		
5、每个独立的线程有一个程序运行的入口、顺序执行序列和程序的出口，但是线程不能够独立执行，必须依存在应用程序中，由应用程序提供多个线程执行控制
<!--more-->
##### 线程与协程的区别 #####

1、一个线程可以有多个协程，一个进程也可以单独拥有多个协程

2、线程进程都是同步机制，而协程则是异步

3、协程能保留上一次调用的状态，每次过程重入时，就相当于进入上一次调用的状态

### 同步阻塞 ###

##### 多线程 #####

最早的服务器端程序都是通过多进程、多线程来解决并发IO的问题。一个请求创建一个进程，然后子进程进入循环同步堵塞地与客户端连接进行交互，收发处理数据	。用多线程模式实现非常简单，线程中可以直接向某一个客户端连接发送数据。

步骤
			
1、创建一个socket

2、进入while循环，阻塞在进程accept操作上，等待客户端连接进入
			
3、主进程在多进程模型下通过fork创建子进程（多线程模型可以创建子线程）
			
4、子进程/线程创建成功后进入while循环，阻塞在recv调用上，等待客户端向服务端发送数据
			
5、收到数据后服务器程序进行处理然后使用send向客户端发送响应
			
6、当客户端连接关闭时，子进程/线程退出并销毁所有资源。主进程/线程会回收此子进程/线程

缺点

- 严重依赖进程的数量解决并发问题

- 启动大量进程会带来额外的进程调度消耗


		$sockserv = stream_socket_server('tcp://host:port', $errno, $errst);
		for($i = 0; $i < 5; $i++){
			if(pcntl_fork() == 0){
				while(true){
					$conn = stream_socket_accept($sockserv);
					if($conn == false)
						continue;
					$request = fread($conn, 9000);
					$response = 'hello';
					fwrite($conn, $response);
					fclose($conn);
				}
				exit(0);
			}
		}


### 异步非阻塞 ###

现在各种高并发异步IO的服务器程序都是基于epoll实现。
		
IO复用异步非阻塞程序使用经典Reactor模型，Reactor是反应堆的意思，本身不处理任何数据收发，只是可以监视一个socket句柄的时间变化。

##### Reactor模型 #####

- `add` 添加一个socket到reactor

- `set` 修改socket对应的事件监听，可以设置监听的类型，如可读可写

- `del` 从Reactor中移除，不在监听事件

- `callback` 事件发生后回调指定的函数，一般在add/set时指定

nginx 多线程Reactor；swoole 多线程Reactor 多线程Worker。

	$reactor = new Reactor();
	$sockserv = steam_socket_server('tcp://host:port');
	$reactor->add($sockserv, EV_READ, function() use($sockserv, $reactor){
		$sockcli = steam_socket_accept($sockserv);
		$reactor->add($sockcli, EV_READ, function() use($sockcli, $reactor){
			$request = fread($sockcli, 10000);
			$reactor->add($sockcli, EV_WRITE, function() use($sockcli, $request){
				fwrite($sockcli, 'test');
				$reactor->del($sockcli);
				fclose($sockcli);
			});
		});
	});

### 并发编程实践 ###

##### swoole扩展 #####

php的异步、并行、高性能网络通信引擎，使用纯c编写，提供了php的异步多线程服务器，异步tcp/udp网络客户端，异步mysql，异步redis，数据库连接池，asynctask，消息队列，毫秒定时器，异步文件读写，异步dns查询。

除了异步io的支持之外，swoole为php多线程的模式设计了多个并发数据结构和IPC通信机制，大大简化多进程并发编程工作。

##### swoole的异步mysql实现 #####

	$db = new Swoole\MySQL;
	$server = [
		'host' => '',
		'user' => '',
		'password' => '',
		'database' => ''
	];
	$db->connect($server, function($db, $result){
		$db->query("show tables", function(Swoole\MySQL $db, $result){
			// do something
		});
	});

### 消息队列 ####

邮件发送、应用解耦、流量削峰、日志处理、消息通知

### 接口并发请求 ###

	curl_multi_init