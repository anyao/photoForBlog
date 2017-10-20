---
title: php面试题整理-数据结构
date: 2017-10-10 11:00:50
tags:
	- php面试
---
##### array #####
	
使用连续的内存来存储、数组中的所有元素必须是相同的类型或类型的衍生（同质数据结构）、元素可以通过下标直接访问

##### linkedlist #####

元素之间的关系是一对一的关系（除了第一个和最后一个元素，其他元素都是首尾相接）、顺序存储结构和链式存储结构两种存储方式

##### stack #####
	
存储数据是先进后出的，栈只有一个出口，只能从栈顶部增加或者移除元素
<!-- more -->
##### heap #####
	
堆，二叉堆，近似完全二叉树的数据结构
	
子节点的键值或者索引总是小于它的父节点、每个节点的左右子树有是一个二叉堆、根节点最大的堆叫做最大堆或者大根堆、最小的叫最小堆或者小根堆

##### list #####
	
线性表是一个序列、0个元素构成的线性表是空表、第一个元素无先驱、最后一个元素无后继、其他元素都只有一个先驱和后继、有长度，长度是元素个数，长度有限

##### doubly-linked-list #####

双向链表，每个元素都是一个对象，每个对象有一个关键字key和两个指针（next和prev）

##### queue #####
	
先进先出（FIFO）、并发中使用，可以安全将对象从一个任务传给另一个任务

##### set #####
	
保存不重复的元素

##### map #####

关联数组、字典或者键值对

##### graph #####
	
通常使用邻接矩阵和邻接表表示、前者易实现但是对于稀疏矩阵会浪费较多空间、后者使用链表链表的方式存储信息但是对于图搜索时间复杂度较高

### eg. ###

1、php实现双向队列
	
	array_shift 
	array_unshift 
	array_pop
	array_push

2、1,1,2,3,5,8,13,21,34....求第30位的数是多少，用伪代码实现。
		
	$arr = [1, 1];
	for ($i = 2; $i < 30; $i++) {
		$arr[$i] = $arr[$i-1] + $arr[$i-2];
	}
	var_dump($arr);

3、open_door -> OpenDoor; make_by_id -> MakeById

	function strHandle($str){
		$return = '';
		$arr =explode('_', $str);
		foreach($arr as $val)
			$return .= ucfirst($val);
		return $return;
	}

3、abcdefg -> gfedcba
		function str_rev($str){
			for($i=0; true; $i++)
				if(!isset($str[$i]))
					break;
			
			$return = '';
			for($j=$i-1; $j>=0; $j--)
				$return .= $str[$j];
			return $return;
		}

4、 array_merge

	function array_mer(){
		$return = [];
		$arrays = func_get_args();
		foreach($arrays as $arr)
			if(is_array($arr))
				foreach($arr as $val)
					$return[] = $val;
		return $return;
	}