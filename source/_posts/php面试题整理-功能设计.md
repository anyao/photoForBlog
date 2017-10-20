---
title: php面试题整理-功能设计
date: 2017-10-09 14:49:19
tags:
	- php面试
---
- 数据表设计 

- 表创建语句

- 选择php连接数据库方式
	
###eg. ###

在线留言本，实现在线留言功能，信息存储到数据库。设计数据表内容以及使用php编码完成。

- 表创建语句
<!-- more -->

		CREATE TABLE message(
			id INT UNSIGNED NOT NULL AUTO_INCREMENT PRIMARY KEY,
			title VARCHAR(120) NOT NULL DEFAULT '',
			content VARCHAR(255) NOT NULL DEFAULT '',
			created_at INT NOT NULL DEFAULT '0',
			user_name VARCHAR(32) UNSIGNED NOT NULL DEFAULT '',
			KEY message_user_name(user_name)
		)ENGINE=InnoDB DEFAULT CHARSET=utf8;

- 连接方式 
	
	PDO / MYSQLI / MYSQL
			

		TRY {
			$dsn = 'mysql:dbname=test;host=localhost';
			$attr = [
				PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION
			];
			$pdo = new PDO($dsn, $username, $password, $attr);
			$sql = 'SELECT id,title,content FROM message where user_name=:username';
			$stmt = $pdo->prepare($sql);
			$stmt->execute([':user_name' => $user_name]);
			$result = $stmt->fetchAll(PDO::FETCH_ASSOC);
	
			if ($result) {
				exit('success');
			} else {
				exit('fail');
			}
		} CATCH (PDOException $e) {
			echo $e->getMessage();
		}

### eg. ### 

设计无限分类：id title pid 递归 (path order by)
