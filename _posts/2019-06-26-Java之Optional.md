---
layout: post
title: 'Java之Optional'
date: 2019-06-26
author: Sealer
cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: java Optional null NullPointerException  

---

## Optional
  1. **_Optional用法之优雅判空1-获取用户名_**
  
  　　之前写法：
  
  ```java
  public static String getName(User user) {
      if (user == null) {
          return "Unknown";
      }
      
      return u.getName();
  }
  ```
  
  　　现在Optional写法：
  ```java
  public static String getName(User user) {
      Optional.ofNullable(user)
          .map(u -> u.getName())
          .orElse("Unknown");
  }
  ```
  
  2. **_Optional用法之优雅判空2-获取第一名姓名_**
  
  　　之前写法：
  
  ```java
  public static String getFirstOneName(Test test) throws IllegalArgumentException {
      if (test !== null) {
          TestResult result = test.getResult();
          if (result != null) {
              Student s = result.getFirstOne();
              if (s != null) {
                  String name = s.getName();
                  return name;
              }
          }
      }
      throw new IllegalArgumentException("The value of param test isn't available.");
  }
  ```
  　　现在Optional写法：
  ```java
  public static String getFirstOneName(Test test) throws IllegalArgumentException {
      return Optional.ofNullable(test)
          .map(t -> t.getResult())
          .map(r -> r.getFirstOne())
          .map(s -> s.getName())
          .orElse(() -> throw new IllegalArgumentException("The value of param test isn't available."));
  }
  ```
  
