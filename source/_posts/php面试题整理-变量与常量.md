---
title: php面试题整理-变量与常量
date: 2017-10-10 14:22:50
tags:
    - php面试
---
- **引用变量**
    在 PHP 中，变量和值是两个概念。PHP是一种弱类型语言，值在PHP的内部（zend引擎），被存放在一个zval结构体中，这个结构体中，除了包含了值的类型，数据外，还包含两个字节的信息。一个是 `is_ref`，是一个bool值，用来标识这个值是否是一个**强制引用**。第二个额外字节 是`refcount`，用来表示指向这个值的变量（也称符号即symbol）的个数。如果refcount为0，那么这个值就可以被回收了。
    <!-- more -->

    所以，引用意味着用不同的名字访问同一个内容（值）；
    
    + ZVAL记录变量引用情况，可以使用 `xdebug_debug_zval()` 函数来输出 `refcount` 和 `is_ref` 的值。
    
    + 写时复制（COPY ON WRITE）

        ```
        $i = 4;   
        // i:(refcount=1, is_ref=0),int 4
        
        $j = $i;   
        // i:(refcount=2, is_ref=0),int 4
        // j:(refcount=2, is_ref=0),int 4
        
        $j = 5; 
        // i:(refcount=1, is_ref=0),int 4
        // j:(refcount=1, is_ref=0),int 5
        ```

        1、 内核创建一个zval指针，并且为其以堆的方式开辟空间，让指针指向这个空间，将zval中的成员引用计数置为1，类型标记为整形，并且申请一个zvalue_value指针，同样以堆的方式以其开辟空间,同时将该联合体中的zval赋值为4,并且在symbal_table的hash表中记录变量i和zval指针的映射关系；
        
        2、没有在申请内存空间，在zval的成员中引用计数标记为2；
        
        3、内核重新创建zval指针，重复下 1 操作，重点是将旧的zval引用计数标记为1。

        P.S. 对象本身就是引用传递,没有COW（想复制可以用clone。

    + 取消引用
        
        unset 只会取消引用，不会销毁空间。

- **常量**。

    + 字符串的定义方式及区别

        定义：单引号、双引号、heredoc/newdoc
        
        区别：单引号不能解析转义字符，只可解析单引号、反斜线本身

    + const和define的区别 
        const更快，是语言结构；define是函数
        define不能用于类常量，const可以
        
    P.S. 常量一经定义，不能修改和删除。__FILE__返回文件的路径和文件名
 
- **超全局变量**
    
    ```
    //  http://localhost/index.php/foo?id=666&name=ann
    
    $_SERVER['SERVER_ADDR']     // 服务器IP地址
    $_SERVER['REMOTE_ADDR']     // 客户端IP地址 
    $_SERVER['QUERY_STRING']    // get时，返回查询字符串 id=666&name=ann
    $_SERVER[' REQUEST_URI']   // 完整URL地址（不包括主机名）/index.php/foo?id=666&name=ann
    $_SERVER['PATH_INFO']   // URL中的路径部分 /foo
    ```

- **运算符**
    
    递增/递减 > ! > 算数运算符 > 大小比较 > (不)相等比较 > 引用 > 位运算符(^)> 位运算符(|) > 逻辑与 > 逻辑或 > 三目 > 赋值 > and > xor > or

- **遍历数组的三种方式**
    
    for 索引
   
    foreach 索引、关联
    
    while、list()、each() 索引、关联
    
    while、list()、each()组合不会reset() 重置指针，而foreach会

    使用elseif的一个基本原则，把优先范围小的条件放在前面处理
    
    switch后面的表达式的数据类型只能是整形、浮点类型或者字符串

- **静态变量**
    
    静态变量仅在局部函数域中存在，但当程序执行离开此作用域是，其值并不会消失。
        
    仅初始化一次，需赋值

- **外部文件导入**

    include/require包含并运行指定文件

    如果给出路径名按照路径来找，否则从include_path中查找；如果include_path中也没有，则从调用脚本文件所在目录和当前工作目录下寻找

    加载过程中未找到文件则include会warning；require会fatal error