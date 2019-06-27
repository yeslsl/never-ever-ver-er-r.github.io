---
layout: post
title: 'Java之Optional'
date: 2019-06-26
author: Sealer
cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: java Optional null NullPointerException  

---

## Optional

  　　Optional是JDK8开始引入的一种优雅解决NPE问题方案，尤其对方法的多级串联调用提供了更好的体验。
  
  　　Optional一共提供了两种构造方法，均为private权限，如下：
  ```java
  private Optional() {
      this.value = null;
  }
  
  private Optional(T value) {
      this.value = Objects.requireNonNull(value);
  }
  ```
  
  　　该类还提供了一个泛型成员变量， 修饰为private final, 即该成员变量只能通过上述的单参构造方法赋值，其代表需优雅解决NPE的目标对象。定义如下：
  ```java
  private final T value;
  ```
  
  　　该类还提供了一个自身类型Optional的对象EMPTY,意为value为null。如下：
  ```java
  private static final Optional<?> EMPTY = new Optional<>();
  ```
  
  　　除此之外，该类提供了如下十余种方法， 接下来依次分析介绍：
  ```java
  public static<T> Optional<T> empty();
  public static <T> Optional<T> of(T value);
  public static <T> Optional<T> ofNullable(T value);
  public T get();
  public boolean isPresent();
  public void ifPresent(Consumer<? super T> consumer);
  public Optional<T> filter(Predicate<? super T> predicate);
  public<U> Optional<U> map(Function<? super T, ? extends U> mapper);
  public<U> Optional<U> flatMap(Function<? super T, Optional<U>> mapper);
  public T orElse(T other);
  public T orElseGet(Supplier<? extends T> other);
  public <X extends Throwable> T orElseThrow(Supplier<? extends X> exceptionSupplier) throws X;
  ```
  
  　　TODO 未完待续。。。
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
  
  　　Optional写法：
  ```java
  public static String getName(User user) {
      User u = Optional.ofNullable(user)
              .orElse("Unknown");
      return u.getName();
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
                  return s.getName();
              }
          }
      }
      throw new IllegalArgumentException("The value of param test isn't available.");
  }
  ```
  　　Optional写法：
  ```java
  import lombok.AllArgsConstructor;
  import lombok.Getter;
  import lombok.NoArgsConstructor;
  
  import java.util.Arrays;
  import java.util.List;
  import java.util.Optional;
  
  /**
   * 功能描述： $ Main
   *
   * @author sealer
   * @email 1178884049@qq.com
   * @date 2019年06月27日 11时00分39秒
   */
  public class Main {
      public static void main(String[] args) {
          Test test = new Test(new TestResult());
          String name = getFirstOneName(test);
          System.out.println(name);
      }
  
      public static String getFirstOneName(Test test) throws IllegalArgumentException {
          Student student =  Optional.ofNullable(test)
                  .map(t -> t.getResult())
                  .map(r -> r.getFirstOne())
                  .orElseThrow(() -> new IllegalArgumentException("The value of param test isn't available."));
          return student.getName();
      }
  }
  
  @Getter
  @AllArgsConstructor
  class Test {
      private TestResult result;
  }
  
  @Getter
  class TestResult {
      private List<Student> students = Arrays.asList(new Student());
  
      public Student getFirstOne() {
          return students.get(0);
      }
  }
  
  @Getter
  @NoArgsConstructor
  @AllArgsConstructor
  class Student {
      private String name;
  }

  ```
  
