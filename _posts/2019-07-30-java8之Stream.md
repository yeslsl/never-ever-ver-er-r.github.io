---
layout: post
title: 'java8之Stream'
date: 2019-07-30
author: Sealer
cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: java8 stream 流操作 流 

---

参考链接:[IBM 之Java 8 中的 Streams API 详解](https://www.ibm.com/developerworks/cn/java/j-lo-java8streamapi/ 
"https://www.ibm.com/developerworks/cn/java/j-lo-java8streamapi/")

## Stream简介
　　Java 8 中的 Stream 是对**_集合（Collection）_**对象功能的增强，它专注于对集合对象进行各种非常便利、
高效的聚合操作（aggregate operation），或者大批量数据操作 (bulk data operation)。Stream API 借
助于同样新出现的 Lambda 表达式，极大的提高编程效率和程序可读性。同时它提供串行和并行两种模式进行
汇聚操作，并发模式能够充分利用多核处理器的优势，使用 fork/join 并行方式来拆分任务和加速处理过程。

## Stream的构造方式
　　有多种方式生成 Stream Source：
* 从 Collection 和数组

||
|:---:|
|Collection.stream()                   |
|Collection.parallelStream()           (并行流， 可以发挥多核优势， 但不保证元素为原来顺序！)|
|Arrays.stream(T array) or Stream.of() |
|从 BufferedReader                      |
|java.io.BufferedReader.lines()        |

* 静态工厂

||
|:---:|
|java.util.stream.IntStream.range()|
|java.nio.file.Files.walk()|

* 自己构建

||
|:---:|
|java.util.Spliterator|

* 其它

||
|:---:|
|Random.ints()|
|BitSet.stream()|
|Pattern.splitAsStream(java.lang.CharSequence)|
|JarFile.stream()|

**_示例：_**

```java
// 1. Individual values 多个单值构造
Stream stream = Stream.of("a", "b", "c");

// 2. Arrays 数组构造(Stream和Arrays都有提供方法)
String [] strArray = new String[] {"a", "b", "c"};
stream = Stream.of(strArray);
stream = Arrays.stream(strArray);

// 3. Collections 集合构造
List<String> list = Arrays.asList(strArray);
stream = list.stream();
```

## Stream的包装类型
　　需要注意的是，对于基本数值型，目前有三种对应的包装类型 Stream：

　　IntStream、LongStream、DoubleStream。当然我们也可以用 Stream&lt;Integer&gt;、
Stream&lt;Long&gt;&gt;、Stream&lt;Double&gt;，但是 boxing 和 unboxing 会很耗时，所以特别为这三种
基本数值型提供了对应的 Stream。

　　Java 8 中还没有提供其它数值型 Stream，因为这将导致扩增的内容较多。而常规的数值型聚合运算可以通过
上面三种 Stream 进行。

## Stream解决的问题

* 集合元素排序
* 集合元素过滤
* 集合元素个数统计
* 集合元素映射转换(同类型或异类型)
* 集合元素遍历
* ......

## 流的常用操作
　　流的操作类型分为两种：
  * Intermediate：一个流可以后面跟随零个或多个 intermediate 操作。其目的主要是打开流，做出某种程度的
  数据映射/过滤，然后返回一个新的流，交给下一个操作使用。这类操作都是惰性化的（lazy），就是说，仅仅调
  用到这类方法，并没有真正开始流的遍历。
  * Terminal：一个流只能有一个 terminal 操作，当这个操作执行后，流就被使用“光”了，无法再被操作。所以
  这必定是流的最后一个操作。Terminal 操作的执行，才会真正开始流的遍历，并且会生成一个结果，或者一个 
  side effect。
  
  　　还有一种操作被称为 short-circuiting。用以指：
  * 对于一个 intermediate 操作，如果它接受的是一个无限大（infinite/unbounded）的 Stream，但返回一个
  有限的新 Stream。
  *对于一个 terminal 操作，如果它接受的是一个无限大的 Stream，但能在有限的时间计算出结果。
  　　当操作一个无限大的 Stream，而又希望在有限时间内完成操作，则在管道内拥有一个 short-circuiting 操作
  是必要非充分条件。
  
**_中间操作_**

|操作名称|操作类型|操作解释|
|:---:|:---:|:---:|
|map (mapToInt, flatMap等)|Intermediate(中间操作)|元素映射，将集合中的元素映射为另一个元素|
|filter|Intermediate(中间操作)|过滤操作， 过滤掉部分不符合条件的元素|
|distinct|Intermediate(中间操作)|唯一操作，将重复的元素只保留一个|
|sorted|Intermediate(中间操作)|排序，对元素按一定规则进行排序|
|peek|Intermediate(中间操作)|aaa|
|limit|Intermediate(中间操作)|截取元素中的前几个|
|skip|Intermediate(中间操作)|aaa|
|parallel|Intermediate(中间操作)|aaa|
|sequential|Intermediate(中间操作)|aaa|
|unordered|Intermediate(中间操作)|aaa|

---

**_终端操作_**

|操作名称|操作类型|操作解释|
|:---:|:---:|:---:|
|forEach|Terminal(终端操作)|对流中的元素进行遍历|
|forEachOrdered|Terminal(终端操作)|aaa|
|toArray|Terminal(终端操作)|将流转换为数组|
|reduce|Terminal(终端操作)|归约操作，将流中的所有元素归约成一个元素， 参见：[java stream api中的reduce方法使用](https://www.jianshu.com/p/3b0fbcc9f24d, "https://www.jianshu.com/p/3b0fbcc9f24d")|
|collect|Terminal(终端操作)|将流通过指定的Collector转换为集合, 各种Collector实现见 [java.util.stream.Collectors.java](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html "https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html")|
|min|Terminal(终端操作)|取流中元素最小的一个返回|
|max|Terminal(终端操作)|取流中元素最大的一个返回|
|count|Terminal(终端操作)|对流中元素的个数进行统计|
|anyMatch|Terminal(终端操作)|找到一个匹配的元素即返回|
|allMatch|Terminal(终端操作)|找到所有匹配要求的元素才返回|
|noneMatch|Terminal(终端操作)|aaa|
|findFirst|Terminal(终端操作)|找到第一个撇皮条件的元素返回|
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
