---
title: memcached的一致性哈希分布式算法
date: 2017-10-11 11:43:26
tags:
	- memcached
	- hash
---
##### 根据余数计算分散 #####

根据服务器台数的余数进行分散：求得键的整数哈希值，再除以服务器台数，根据其余数来选择服务器。

余数计算的方法简单，数据的分散性也相当优秀，但也有其缺点。那就是当添加或移除服务器时，缓存重组的代价相当巨大。添加服务器后，余数就会产生巨变，这样就无法获取与保存时相同的服务器，从而影响缓存的命中率。

eg. 由 N 台服务器变为 N-1 台——每 N*(N-1) 个数中，求余相同的数只有 N 个数字。所以命中率在服务器 down 的短期内急剧下降至 N/(N*(N-1)) = 1/(N-1)。
<!-- more -->
##### Consistent Hashing #####

首先求出 memcached 服务器（节点）的哈希值，并将其配置到 0～2^32 的圆（continuum）上。然后用同样的方法求出存储数据的键的哈希值，并映射到圆上。然后从数据映射到的位置开始顺时针查找，将数据保存到找到的第一个服务器上。如果超过 2^32 仍然找不到服务器，就会保存到第一台 memcached 服务器上。

如果添加一台 memcached 服务器，余数分布式算法由于保存键的服务器会发生巨大变化。而影响缓存的命中率，但 Consistent Hashing 中，只有在增加服务器的地点逆时针方向的第一台服务器上的键会受到影响。

因此，Consistent Hashing 最大限度地抑制了键的重新分布。而且，有的 Consistent Hashing 的实现方法还采用了虚拟节点的思想。使用一般的 hash 函数的话，服务器的映射地点的分布非常不均匀。

因此，使用虚拟节点的思想，为每个物理节点（服务器）分配 100～200个点。这样就能抑制分布不均匀，最大限度地减小服务器增减时的缓存重新分布。
	
	```
	/** 
     * Get a list of targets for the resource, in order of precedence. 
     * Up to $requestedCount targets are returned, less if there are fewer in total. 
     * 
     * @param string $resource 
     * @param int $requestedCount The length of the list to return 
     * @return array List of targets 
     * @comment 查找当前的资源对应的节点, 
     *          节点为空则返回空,节点只有一个则返回该节点, 
     *          对当前资源进行hash,对所有的位置进行排序,在有序的位置列上寻找当前资源的位置 
     *          当全部没有找到的时候,将资源的位置确定为有序位置的第一个(形成一个环) 
     *          返回所找到的节点 
     */  
    public function lookupList($resource, $requestedCount)  
    {  
        if (!$requestedCount)  
            throw new Flexihash_Exception('Invalid count requested');  
  
        // handle no targets  
        if (empty($this->_positionToTarget))  
            return array();  
  
        // optimize single target  
        if ($this->_targetCount == 1)  
            return array_unique(array_values($this->_positionToTarget));  
  
        // hash resource to a position  
        $resourcePosition = $this->_hasher->hash($resource);  
  
        $results = array();  
        $collect = false;  
  
        $this->_sortPositionTargets();  
  
        // search values above the resourcePosition  
        foreach ($this->_positionToTarget as $key => $value)  
        {  
            // start collecting targets after passing resource position  
            if (!$collect && $key > $resourcePosition)  
            {  
                $collect = true;  
            }  
  
            // only collect the first instance of any target  
            if ($collect && !in_array($value, $results))  
            {  
                $results []= $value;  
            }  
  
            // return when enough results, or list exhausted  
            if (count($results) == $requestedCount || count($results) == $this->_targetCount)  
            {  
                return $results;  
            }  
        }  
  
        // loop to start - search values below the resourcePosition  
        foreach ($this->_positionToTarget as $key => $value)  
        {  
            if (!in_array($value, $results))  
            {  
                $results []= $value;  
            }  
  
            // return when enough results, or list exhausted  
            if (count($results) == $requestedCount || count($results) == $this->_targetCount)  
            {  
                return $results;  
            }  
        }  
  
        // return results after iterating through both "parts"  
        return $results;  
    }  
	```

[Flexihash](http://pan.baidu.com/s/1c22bGWg)