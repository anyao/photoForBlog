---
title: php的异常处理
date: 2017-10-01 15:22:24
tags:
    - 读书笔记
    - php核心技术与最佳实践
    - 异常处理
---
php只有手动抛出异常才能捕获异常。，或者是有内建的异常机制时，会先触发错误，再捕获异常。
<!-- more -->

- **定义**
    
    ```
    class emailException extends exception{
        function __toString(){
            ...
        }
    }
    ```
    
- **抛出**

    ```
    if(empty($reginfo['email']))
        throw new emailException('the email is empty');
    ```

- **处理**

    ```
    try{
        reg(['email' => 'annyao@icloud.com']);
    }catch(emailException $ee){
        echo $ee;
    }catch(Exception $e){
        echo $e->getTraceAsString();
    }
    ```

    exception作为超类应该最后被捕获。不然，捕获这个异常超类后，后面的捕获就终止了。
    
    不符合要求的情况都视作业务异常，和通常意义上的代码异常相区别。从业务角度讲，异常偏重于保护业务数据的一致性，并且强调对异常业务的处理。

- **异常处理的时机**

    两种捕获异常的方式：异常发生时立刻捕获；分散抛异常集中捕获。

    如果业务很重要，那么异常越早处理越好；如果不那么重要，并且在单一入口、MVC风格的应用中，为了保持代码流程的同意，则采用后一种。