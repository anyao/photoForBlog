---
title: 替换Angularjs解析符
date: 2017-09-06 23:11:46
tags:
    - 日常整理
    - angularjs
---
解决与 Laravel 解析符相冲突的情况。 
```
angular.module('appname', [])
    .config(function($interpolateProvider){
        $interpolateProvider.startSymbol('[:');
        $interpolateProvider.endSymbol(':]');
})
```