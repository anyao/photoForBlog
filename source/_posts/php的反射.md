---
title: php的反射
date: 2017-10-01 13:55:28
tags:
    - 读书笔记
    - 反射
    - php核心技术与最佳实践
---
- **反射API**

    ```
    // 获取对象的属性列表
    $reflect = new RlectionObject($obj);
    $props = $reflect->getProperties();
    foreach($props as $prop){
    print $prop->getName();
    }
    
    // 获取对象方法列表
    $m = $reflect->getMethod();
    foreach($m as $prop){
    print $prop->getName();
    }
    ```
<!-- more -->
- **class函数**

    ```
    // 返回对象属性的关联函数
    var_dump(get_object_vars($obj));
    // 类属性
    var_dump(get_class_vars(get_class($class)));
    // 返回由类的方法名组成的数组
    var_dumo(get_class_methods(get_class($class)));
    // 获取对象属性列表所属的类
    echo get_class($obj);
    ```
    
在平常的开发中，用到反射的地方不多：一是对对象进行调试；另一个是获取类的信息。很多时候，善用反射能保持代码的优雅和简介，但反射也会破坏类的封装性。

