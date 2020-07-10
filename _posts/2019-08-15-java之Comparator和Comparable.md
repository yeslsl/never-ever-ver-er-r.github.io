---
layout: post
title: 'java之Comparator和Comparable'
date: 2019-06-20
author: Sealer
cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: java Comparator Comparable  

---

## 比较

|比较项|Comparator|Comparable|
|:---:|:---:|:---:|
|接口 or 类|接口|接口|
|签名|public interface Comparator&lt;T&gt;|public interface Comparable&lt;T&gt;|
|比较用方法签名|int compare(T o1, T o2);|public int compareTo(T o);|
|比较规则|1. 返回值小于0: o1 < o2; 2. 返回值等于0: o1 = o2; 3. 返回值大于0: o1 > o2.|1. 返回值小于0: this < o; 2. 返回值等于0: this = o; 3. 返回值大于0: this > o.|
|特点|"外部比较器", 表示"某个类的比较规则"， 可用于比较不支持内部比较器的类（即未实现Comparable接口的类），简言之将比较规则外置。|"内部比较器", 表示"该类支持排序"， 可用于Collections.sort()等排序方法|
