---
title: Predis
date: 2017-09-07 15:52:17
tags:
    - predis
---
- **predis 与 phpredis**
>  redis官方推荐的php客户端是predis和phpredis。前者是完全使用php代码实现的原生客户端，后者则是用c编写的php拓展。在功能上二者区别不大，就性能而言后者更胜一筹。
> <!-- more -->
> 虽然predis的性能逊于phpredis，但除非执行大量的redis命令，否则很难区分二者的性能。而且实际应用中执行redis的命令的开销更多在网络传输上，单纯注重客户端的性能的意义不大。
> 
> ——摘自《redis入门指南》第五章

- **安装**
    ```
    composer require predis/predis dev-master
    ``` 

- **使用**
    ```
    <?php
            require './vendor/autoload.php';
            Predis\Autoloader::register();
            $redis = new Predis\Client([
                'host' => '127.0.0.1',
                'port' => '6379',
                'database' => '0',
                'password' => 'adminpass'
            ]);
            $redis->set('foo', 'bar');
            $value = $redis->get('foo');
    ```

- **P.S.**
    [https://packagist.org/packages/predis/predis](https://packagist.org/packages/predis/predis)