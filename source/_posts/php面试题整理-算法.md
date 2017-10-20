---
title: php面试题整理-算法
date: 2017-10-09 16:09:19
tags:
	- php面试
---
### 算法概念 ###

解决特定问题求解步骤的描述，在计算机中表现为指令的有限序列，并且每条指令表示一个或多个操作。

### 五个特征 ###

有穷性、确切性、输入项、输出项、可行性

### 算法评定 ###

##### 时间复杂度 #####

- 得出算法的计算次数公式

- 用常数1来取代所有时间中的所有加法常数

- 在修改后的运行次数函数中，只保留最高阶项

- 如果最高阶存在且不是1，则去除与这个项相乘的函数
<!--more-->
- eg.


		常数阶 O(1) 
		线性阶 O(n)
		平方阶 O(n^2) / O(n^3)
		对数阶 O(log2n)
		O(1) > O(log2n) > O(n) > O(nlog2n) > O(n^3) > O(2^n) > O(n!) > O(n^n)


##### 空间复杂度 #####

- 算法需要消耗的内存空间，记作 `S(n) = O(f(n))`

- 包括程序代码所占用的空间，输入数据所占用的空间和辅助变量所占用的空间

- 计算和表示方法与时间复杂度类似，一般用复杂度渐近性来表示

- 有时用空间换取时间

- 交换的空间复杂度是 `O(1)`

### 排序 ###

##### 冒泡排序 #####

两两相邻的数进行比较，如果反序就交换，否则不交换。时间复杂度 最坏 `O(n^2)` 平均` O(n^2)`。空间复杂度 `O(1)`。

	function swap(array &$arr,$a,$b){
	    $temp = $arr[$a];
	    $arr[$a] = $arr[$b];
	    $arr[$b] = $temp;
	}

	for ($i = 0; $c = count($arr); $i <= $c; $i++) {
		for ($j = 0; $j < $c - $i; $j++) {
			if ($arr[$j] > $arr[$j+1])
				swap($arr, $j, $j+1)
		}
	}

	// 改进
	function BubbleSort1(array &$arr){
	    $length = count($arr);
	    $flag = TRUE;
	
	    for($i = 0;($i < $length - 1) && $flag;$i ++){
	        $flag = FALSE;
	        for($j = $length - 2;$j >= $i;$j --){
	            if($arr[$j] > $arr[$j + 1]){
	                swap($arr,$j,$j+1);
	                $flag = TRUE;
	            }
	        }
	    }
	}

##### 直接插入排序 #####
		
每次从无序表中取出第一个元素，把它插入到有序表的合适位置，使有序表仍然有序。时间复杂度 `O(n^2)` 平均 `O(n^2)`。空间复杂度 `O(1)`。
	
[方法一](http://www.cnblogs.com/zemliu/archive/2012/08/16/2643035.html)

	 #指定部分数组元素全部向后移动一位
	 function move(Array $arr, $start = null, $end = null) {
	     if(!isset($start) || $start < 0) $start = 0;
	     if(!isset($end) || $end >= count($arr)) $end = count($arr) - 2;    #最后只能选到倒数第二个元素
	     for($i = $end; $i >= $start; $i--) {
	         $arr[$i + 1] = $arr[$i];
	     }
	     return $arr;
	 }
	 
	 #插入排序,使用同一个数组后移方法实现
	 function insertSort(Array $arr) {
	     for($i = 1; $i < count($arr); $i++) {    #未排序数组,从第二个元素开始
	         $insertEle = $arr[$i];    #待插入元素
	         for($j = 0; $j < $i; $j++) {    #已排序好数组,从第一个元素开始
	             if($arr[$j] > $arr[$i]) {    #按升序排序
	                 $arr = move($arr, $j, $i - 1);    将已排序好数组中大于待插入元素的元素全部后移一位
	                 $arr[$j] = $insertEle;    #插入待插入元素
	                 break;
	             }
	         }
	     }
	     return $arr;
	 }
	 
[方法二](http://blog.csdn.net/baidu_30000217/article/details/53072746)

	function InsertSort(array &$arr){
	    $count = count($arr);
	    //数组中第一个元素作为一个已经存在的有序表
	    for($i = 1;$i < $count;$i ++){
	        $temp = $arr[$i];      //设置哨兵
	        for($j = $i - 1;$j >= 0 && $arr[$j] > $temp;$j --){
	            $arr[$j + 1] = $arr[$j];       //记录后移
	        }
	        $arr[$j + 1] = $temp;      //插入到正确的位置
	    }
	}

##### 希尔排序 #####

把待排序的数据根据增量分层几个子序列，对子序列进行插入排序，直到增量为1，直接进行插入排序；增量的排序，一般是数组的长度的一半，再变为原来增量的一半，直到增量为1。时间复杂度 `O(n^2)` 平均 `O(nlog2n)`。空间复杂度 `O(1)`。

[方法一](http://www.cnblogs.com/firstForEver/p/5133883.html)（每组内其实是冒泡排序）

	function shell_sort(array $arr){
	    // 将$arr按升序排列
	    $len = count($arr);
	    $f = 3;// 定义因子
	    $h = 1;// 最小为1
	    while ($h < $len/$f){
	        $h = $f*$h + 1; // 1, 4, 13, 40, 121, 364, 1093, ...
	    }
	    while ($h >= 1){  // 将数组变为h有序
	        for ($i = $h; $i < $len; $i++){  // 将a[i]插入到a[i-h], a[i-2*h], a[i-3*h]... 之中 （算法的关键
	            for ($j = $i; $j >= $h;  $j -= $h){
	                if ($arr[$j] < $arr[$j-$h]){
	                    $temp = $arr[$j];
	                    $arr[$j] = $arr[$j-$h];
	                    $arr[$j-$h] = $temp;
	                }
	                //print_r($arr);echo '<br/>'; // 打开这行注释，可以看到每一步被替换的情形
	            }
	        }
	        $h = intval($h/$f);
	    }
	    return $arr;
	}

 [方法二](http://blog.csdn.net/baidu_30000217/article/details/53084546)

	function ShellSort(array &$arr)
	{
	    $count = count($arr);
	    $inc = $count;    //增量
	    do {
	        //计算增量
	        //$inc = floor($inc / 3) + 1;
	        $inc = ceil($inc / 2);
	        for ($i = $inc; $i < $count; $i++) {
	            $temp = $arr[$i];    //设置哨兵
	            //需将$temp插入有序增量子表
	            for ($j = $i - $inc; $j >= 0 && $arr[$j + $inc] < $arr[$j]; $j -= $inc) {
	                $arr[$j + $inc] = $arr[$j]; //记录后移
	            }
	            //插入
	            $arr[$j + $inc] = $temp;
	        }
	        //增量为1时停止循环
	    } while ($inc > 1);
	}


##### 选择排序 #####

每次从待排序的数据元素中选出最小（或者最大）的一个元素，存放在序列的原始位置，直到全部待排序的数据的元素排完。时间复杂度 `O(n^2)` 平均 `O(n^2)`。空间复杂度 `O(1)`。


[方法一](http://blog.csdn.net/u013372487/article/details/45581373)

	function SelectSort(array &$arr){
	    $count = count($arr);
	    for($i = 0;$i < $count - 1;$i ++){
	        //记录第$i个元素后的所有元素最小值下标
	        $min = $i;
	        for($j = $i + 1;$j < $count;$j ++){
	            if($arr[$j] < $arr[$min]){
	                $min = $j;
	            }
	        }
	
	        if($min != $i){
	            swap($arr,$min,$i);
	        }
	    }
	}

##### 快速排序 #####

通过一趟排序将要排序的数组分割成独立的两部分，其中一部分的数据都比另一部分的所有数据都要小，然后再按照此方法对两部分数据分别进行快速排序，整个排序过程可以递归完成。时间复杂度 `O(n^2)` 平均 `O(nlog2n)`。空间复杂度 `O(n)`，平均 `O(log2n)`。
	
[方法一](http://blog.csdn.net/baidu_30000217/article/details/53311840)
	
	function swap(array &$arr,$a,$b){
	    $temp = $arr[$a];
	    $arr[$a] = $arr[$b];
	    $arr[$b] = $temp;
	}
	
	function Partition(array &$arr,$low,$high){
	    $pivot = $arr[$low];   //选取子数组第一个元素作为枢轴
	    while($low < $high){  //从数组的两端交替向中间扫描
	        while($low < $high && $arr[$high] >= $pivot){
	            $high --;
	        }
	        swap($arr,$low,$high);	//终于遇到一个比$pivot小的数，将其放到数组低端
	        while($low < $high && $arr[$low] <= $pivot){
	            $low ++;
	        }
	        swap($arr,$low,$high);	//终于遇到一个比$pivot大的数，将其放到数组高端
	    }
	    return $low;   //返回high也行，毕竟最后low和high都是停留在pivot下标处
	}
	
	function QSort(array &$arr,$low,$high){
	    if($low < $high){
	        $pivot = Partition($arr,$low,$high);  //将$arr[$low...$high]一分为二，算出枢轴值
	        QSort($arr,$low,$pivot - 1);   //对低子表进行递归排序
	        QSort($arr,$pivot + 1,$high);  //对高子表进行递归排序
	    }
	}
	
	function QuickSort(array &$arr){
	    $low = 0;
	    $high = count($arr) - 1;
	    QSort($arr,$low,$high);
	}

[优化](http://blog.csdn.net/baidu_30000217/article/details/53312990)

	function Partition(array &$arr,$low,$high){
	    $mid = floor($low + ($high - $low) / 2);    //计算数组中间的元素的下标
	    if($arr[$low] > $arr[$high]){
	        swap($arr,$low,$high);
	    }
	    if($arr[$mid] > $arr[$high]){
	        swap($arr,$mid,$high);
	    }
	    if($arr[$low] < $arr[$mid]){
	        swap($arr,$low,$mid);
	    }
	
	    //经过上面三步之后，$arr[$low]已经成为整个序列左中右端三个关键字的中间值
	    $pivot = $arr[$low];
	
	    $temp = $pivot;
	
	    while($low < $high){   //从数组的两端交替向中间扫描（当 $low 和 $high 碰头时结束循环）
	        while($low < $high && $arr[$high] >= $pivot){
	            $high --;
	        }
	        //swap($arr,$low,$high);	//终于遇到一个比$pivot小的数，将其放到数组低端
	        $arr[$low] = $arr[$high];   //使用替换而不是交换的方式进行操作
	
	        while($low < $high && $arr[$low] <= $pivot){
	            $low ++;
	        }
	        //swap($arr,$low,$high);	//终于遇到一个比$pivot大的数，将其放到数组高端
	        $arr[$high] = $arr[$low];
	    }
	
	    $arr[$low] = $temp;    //将枢轴数值替换回 $arr[$low];
	
	    return $low;   //返回high也行，毕竟最后low和high都是停留在pivot下标处
	}

##### 堆排序 #####

把待排序的元素按照大小在二叉树位置上排序，排序好的元素要满足——父节点的元素要大于等于子节点，这个过程叫堆化过程。如果根节点存放的是最大的数，则叫做大根堆，如果是最小，就叫小根堆，可以把根节点拿出来，然后再堆化，循环到最后一个节点。时间复杂度 `O(nlog2n)` 平均 `O(nlog2n)`。空间复杂度 `O(1)`。	

[方法一](http://blog.csdn.net/baidu_30000217/article/details/53087079)（《大话数据结构》P397）

	// 调整 $arr[$start]的关键字，使$arr[$start]、$arr[$start+1]...$arr[$end]成为一个大根堆（根节点最大的完全二叉树）
	function HeapAdjust(array &$arr,$start,$end){
	    $temp = $arr[$start];
	    for($j = 2 * $start + 1;$j <= $end;$j = 2 * $j + 1){
	        if($j != $end && $arr[$j] < $arr[$j + 1]){
	            $j ++; //转化为右孩子
	        }
	        if($temp >= $arr[$j]){
	            break;  //已经满足大根堆
	        }
	        //将根节点设置为子节点的较大值
	        $arr[$start] = $arr[$j];
	        //继续往下
	        $start = $j;
	    }
	    $arr[$start] = $temp;
	}
	
	function HeapSort(array &$arr){
	    $count = count($arr);
	    //先将数组构造成大根堆（由于是完全二叉树，所以这里用floor($count/2)-1，下标小于或等于这数的节点都是有孩子的节点)
	    for($i = floor($count / 2) - 1;$i >= 0;$i --){
	        HeapAdjust($arr,$i,$count);
	    }
	    for($i = $count - 1;$i >= 0;$i --){
	        //将堆顶元素与最后一个元素交换，获取到最大元素（交换后的最后一个元素），将最大元素放到数组末尾
	        swap($arr,0,$i);  
	        //经过交换，将最后一个元素（最大元素）脱离大根堆，并将未经排序的新树($arr[0...$i-1])重新调整为大根堆
	        HeapAdjust($arr,0,$i - 1);
	    }
	}

##### 归并排序 （理想）#####

将两个（或两个以上）有序表合并成一个新的有序表，即把待排序序列分成若干个有序的子序列，再把有序的子序列合并为整体有序序列。时间复杂度 `O(nlog2n)` 平均 `O(nlog2n)`。空间复杂度 `O(n)`。

[方法一](http://blog.csdn.net/baidu_30000217/article/details/53363795)（《大话数据结构》P406）
	
	function MergeSort(array &$arr){
	    $start = 0;
	    $end = count($arr) - 1;
	    MSort($arr,$start,$end);
	}

	function MSort(array &$arr,$start,$end){
	    //当子序列长度为1时，$start == $end，不用再分组
	    if($start < $end){
			//将 $arr 平分为 $arr[$start - $mid] 和 $arr[$mid+1 - $end]
	        $mid = floor(($start + $end) / 2);	
			
			//将 $arr[$start - $mid] 归并为有序的$arr[$start - $mid]
	        MSort($arr,$start,$mid);
			
			//将 $arr[$mid+1 - $end] 归并为有序的 $arr[$mid+1 - $end]	
	        MSort($arr,$mid + 1,$end);
			
			//将$arr[$start - $mid]部分和$arr[$mid+1 - $end]部分合并起来成为有序的$arr[$start - $end]			
	        Merge($arr,$start,$mid,$end);       
	    }
	}

	function Merge(array &$arr,$start,$mid,$end){
	    $i = $start;
	    $j=$mid + 1;
	    $k = $start;
	    $temparr = array();
	
	    while($i!=$mid+1 && $j!=$end+1)
	    {
	       if($arr[$i] >= $arr[$j]){
	           $temparr[$k++] = $arr[$j++];
	       }
	       else{
	           $temparr[$k++] = $arr[$i++];
	       }
	    }
	
	    //将第一个子序列的剩余部分添加到已经排好序的 $temparr 数组中
	    while($i != $mid+1){
	        $temparr[$k++] = $arr[$i++];
	    }
	    //将第二个子序列的剩余部分添加到已经排好序的 $temparr 数组中
	    while($j != $end+1){
	        $temparr[$k++] = $arr[$j++];
	    }
	    for($i=$start; $i<=$end; $i++){
	        $arr[$i] = $temparr[$i];
	    }
	}


### 查找 ###

##### 顺序查找 #####

按一定的顺序检查数组中的每一个元素，直到找到所要寻找的特定值为止。时间复杂度 `O(n)` 平均 `O(n)`。空间复杂度 `O(1)`。

《大话数据结构》P297

##### 二分查找 #####

从数组中间元素开始，如果中间元素正好是要查找的元素，搜索结束，如果某一个特定元素大于或者中间元素，则在数组大于或者小于中间元素的那一半中查找，而且跟开始一样从中间开始比较，如果某一部中数组为空，代表找不到。时间复杂度 `O(log2n)` 平均 `O(log2n)`。空间复杂度 迭代 `O(1)` 递归 `O(log2n)`。

[方法一](http://blog.csdn.net/baidu_30000217/article/details/53056977)

	function binsearch($arr,$num){
	    $count = count($arr);
	    $lower = 0;
	    $high = $count - 1;
	    $i = 0;

	    while($lower <= $high){
	
	        $i ++; //计数器
	
	        if($arr[$lower] == $num){
	            return $lower;
	        }
	        if($arr[$high] == $num){
	            return $high;
	        }
	
	        $middle = intval(($lower + $high) / 2);
	        if($num < $arr[$middle]){
	            $high = $middle - 1;
	        }else if($num > $arr[$middle]){
	            $lower = $middle + 1;
	        }else{
	            return $middle;
	        }
	    }
	
	    //返回-1表示查找失败
	    return -1;
	}