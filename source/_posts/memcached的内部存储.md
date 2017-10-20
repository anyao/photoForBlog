---
title: memcached的内部存储
date: 2017-10-11 11:11:39
tags:
	- memcache
	- 读书笔记
---
memcached 默认情况下采用了名为 Slab Allocator 的机制分配、管理内存。

在该机制出现以前，内存的分配是通过对所有记录简单地进行malloc 和 free 来进行的。但是，这种方式会导致内存碎片，加重操作系统内存管理器的负担，最坏的情况下，会导致操作系统比 memcached 进程本身还慢。**Slab Allocator** 就是为解决该问题而诞生的。
<!--more-->
Slab Allocator 的基本原理是按照预先规定的大小，将分配的内存分割成特定长度的块，以完全解决内存碎片问题。

#### Slab Allocation ####

Slab Allocation 的原理相当简单。将分配的内存分割成各种尺寸的块（chunk），并把尺寸相同的块分成组（chunk 的集合）。而且，slab allocator还有重复使用已分配的内存的目的。也就是说，分配到的内存不会释放，而是重复利用。

![Slab Allocation 的构造图](http://img.blog.csdn.net/20171011111642341)

Memcached 根据收到的数据的大小，选择最适合数据大小的 slab。memcached 中保存着 slab 内空闲 chunk 的列表，根据该列表选择 chunk，然后将数据缓存于其中。

##### 缺点 ####

Slab Allocator 解决了当初的内存碎片问题，但新的机制也给 memcached 带来了新的问题。这个问题就是，由于分配的是特定长度的内存，因此无法有效利用分配的内存。例如，将 100 字节的数据缓存到 128 字节的chunk 中，剩余的 28 字节就浪费了。

 因此，为尽量减少内存浪费，memcached 追加了 growth factor 这个选项，默认值为 1.25。