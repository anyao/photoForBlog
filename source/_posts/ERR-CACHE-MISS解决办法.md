---
title: ERR_CACHE_MISS解决办法
date: 2017-09-02 10:53:23
tags:
    - error
---
**SITUATION：** 设备搜索后，根据返回结果 list.php 进入特定设备的详细页面 one.php，但点击后退按钮（上一页）返回 list.php，会出现确认重新提交表单的错误页面 ERR_CACHE_MISS。
<!-- more -->
**REASON：** list.php 本身设置（默认）了 no-store 无缓存。所以返回上一页时，搜索结果页面没有任何值载入，需要重新输入值才会正确显示。

**ACTION：** 在 list.php 放入
```
<?php 
    header("Cache-control: private");
?>
```