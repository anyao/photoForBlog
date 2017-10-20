---
title: CGI、fastCGI与php-fpm
date: 2017-10-09 10:52:49
tags:
	- cgi
	- php-fpm
---
### CGI ###

不同语言的解析器与 Web Server 之间的通信协议。
### fastCGI ###

CGI 效率很低，因为 Web Server 每收到一个请求都会 folk 一个进程，请求结束kill 掉这个进程。高并发时浪费资源，fastCGI的出现解决了这个问题。
<!-- more -->

fastCGI 每次处理完请求之后不会 kill 掉进程，而是保留这个进程。使其一次可以处理多个请求。

### php-fpm ###

**php fastCGI proccess manager** 是 fastCGI 进程管理器，是fastCGI的实现，并且实现了管理进程的功能。

进程包括 master 进程和 worker 进程。master 进程只有一个，负责监听端口，接收来自Web Server的请求；worker 进程一般会有多个，其数量会在fpm中的配置进行定义。每个进程内部会嵌入 php 内部解析器。

worker 来处理代码，master 主要负责监听端口（默认9000），与 Web Server进行接收请求。