---
layout: post
title: 'java8之Stream'
date: 2019-07-30
author: Sealer
cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: java8 stream 流操作 流 

---

参考链接:[IBM 之Java 8 中的 Streams API 详解](https://www.ibm.com/developerworks/cn/java/j-lo-java8streamapi/ "https://www.ibm.com/developerworks/cn/java/j-lo-java8streamapi/")

## Stream简介
　　Java 8 中的 Stream 是对**_集合（Collection）_**对象功能的增强，它专注于对集合对象进行各种非常便利、
高效的聚合操作（aggregate operation），或者大批量数据操作 (bulk data operation)。Stream API 借
助于同样新出现的 Lambda 表达式，极大的提高编程效率和程序可读性。同时它提供串行和并行两种模式进行
汇聚操作，并发模式能够充分利用多核处理器的优势，使用 fork/join 并行方式来拆分任务和加速处理过程。

## Stream的构造方式
1. 通过

## Stream解决的问题

* 集合元素排序
* 集合元素过滤
* 集合元素个数统计
* 集合元素映射转换(同类型或异类型)
* 集合元素遍历

## 流的常用操作

|操作名称|操作类型|操作解释|
|:---:|:---:|:---:|
|map (mapToInt, flatMap等)|Intermediate(中间操作)|aaa|
|filter|Intermediate(中间操作)|aaa|
|distinct|Intermediate(中间操作)|aaa|
|sorted|Intermediate(中间操作)|aaa|
|peek|Intermediate(中间操作)|aaa|
|limit|Intermediate(中间操作)|aaa|
|skip|Intermediate(中间操作)|aaa|
|parallel|Intermediate(中间操作)|aaa|
|sequential|Intermediate(中间操作)|aaa|
|unordered|Intermediate(中间操作)|aaa|

---

|操作名称|操作类型|操作解释|
|:---:|:---:|:---:|
|forEach|Terminal(终端操作)|aaa|
|forEachOrdered|Terminal(终端操作)|aaa|
|toArray|Terminal(终端操作)|aaa|
|reduce|Terminal(终端操作)|aaa|
|collect|Terminal(终端操作)|aaa|
|min|Terminal(终端操作)|aaa|
|max|Terminal(终端操作)|aaa|
|count|Terminal(终端操作)|aaa|
|anyMatch|Terminal(终端操作)|aaa|
|allMatch|Terminal(终端操作)|aaa|
|noneMatch|Terminal(终端操作)|aaa|
|findFirst|Terminal(终端操作)|aaa|
|findAnyiterator|Terminal(终端操作)|aaa|

---

|操作名称|操作类型|操作解释|
|:---:|:---:|:---:|
|anyMatch|Short-circuiting(短路操作)|aaa|
|allMatch|Short-circuiting(短路操作)|aaa|
|noneMatch|Short-circuiting(短路操作)|aaa|
|findFirst|Short-circuiting(短路操作)|aaa|
|findAny|Short-circuiting(短路操作)|aaa|
|limit|Short-circuiting(短路操作)|aaa|
