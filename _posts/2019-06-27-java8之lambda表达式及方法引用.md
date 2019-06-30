---
layout: post
title: 'java8之lambda表达式及方法引用'
date: 2019-06-27
author: Sealer
cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: java java8 lambda 方法引用 method reference  

---

## java8中的函数式接口FunctionalInterface
　　函数式接口(Functional Interface)，就是有且仅有一个抽象方法，但是可以有多个
非抽象方法的接口。**_lambda表达式和方法引用，都是函数式接口的一种具体实现。_**

### 函数式接口可以包含的方法
　　1. 默认方法。

　　2. 静态方法。

　　3. java.lang.Object 里的public方法。

　　getClass hashCode equals toString notify　notifyAll wait wait wait这几个为public方法。

　　registerNatives为私有方法，clone和finalize为protected方法。

### java中提供的函数式接口

　　java8中提供了很多预定义的函数式接口，能满足绝大多数情况的开发。如果
不满足自己的场景， 可以自己定义相应的函数式接口。

　　java8之前就存在的函数式接口：
<style>
.firstTable table th:first-of-type {
	width: 400px;
}
</style>
<div class="firstTable">

| 接口名 |
| :---: |
|java.lang.Runnable|
|java.util.concurrent.Callable|
|java.security.PrivilegedAction|
|java.util.Comparator|
|java.io.FileFilter|
|java.nio.file.PathMatcher|
|java.lang.reflect.InvocationHandler|
|java.beans.PropertyChangeListener|
|java.awt.event.ActionListener|
|javax.swing.event.ChangeListener|

</div>
　　java8新增的函数式接口：

|序号|接口名|接口描述|
|:---:|:---:|:---:|
|1|BiConsumer&lt;T,U>|代表了一个接受两个输入参数的操作，并且不返回任何结果|
|2|BiFunction&lt;T,U,R>|代表了一个接受两个输入参数的操作，并且返回一个结果|
|3|BinaryOperator&lt;T>|代表了一个作用于于两个同类型操作数的操作，并且返回了同类型结果，BiFunction的子接口|
|4|	BiPredicate&lt;T,U>|代表了一个两个参数的boolean值方法|
|5|	BooleanSupplier|代表了boolean值结果的提供方|
|6|	Consumer&lt;T>|代表了接受一个输入参数并且无返回的操作|
|7|	DoubleBinaryOperator|代表了作用于两个double值操作符的操作，并且返回了一个double值的结果。|
|8|	DoubleConsumer|代表一个接受double值参数的操作，并且不返回结果。|
|9|	DoubleFunction&lt;R>|代表接受一个double值参数的方法，并且返回结果|
|10|	DoublePredicate|代表一个拥有double值参数的boolean值方法|
|11|	DoubleSupplier|代表一个double值结果的提供方|
|12|	DoubleToIntFunction|接受一个double类型输入，返回一个int类型结果。|
|13|	DoubleToLongFunction|接受一个double类型输入，返回一个long类型结果|
|14|	DoubleUnaryOperator|接受一个参数同为类型double,返回值类型也为double 。|
|15|	Function&lt;T,R>|接受一个输入参数，返回一个结果。|
|16|	IntBinaryOperator|接受两个参数同为类型int,返回值类型也为int 。|
|17|	IntConsumer|接受一个int类型的输入参数，无返回值 。|
|18|	IntFunction&lt;R>|接受一个int类型输入参数，返回一个结果 。|
|19|	IntPredicate|：接受一个int输入参数，返回一个布尔值的结果。|
|20|	IntSupplier|无参数，返回一个int类型结果。|
|21|	IntToDoubleFunction|接受一个int类型输入，返回一个double类型结果 。|
|22|	IntToLongFunction|接受一个int类型输入，返回一个long类型结果。|
|23|	IntUnaryOperator|接受一个参数同为类型int,返回值类型也为int 。|
|24|	LongBinaryOperator|接受两个参数同为类型long,返回值类型也为long。|
|25|	LongConsumer|接受一个long类型的输入参数，无返回值。|
|26|	LongFunction&lt;R>|接受一个long类型输入参数，返回一个结果。|
|27|	LongPredicate|R接受一个long输入参数，返回一个布尔值类型结果。|
|28|	LongSupplier|无参数，返回一个结果long类型的值。|
|29|	LongToDoubleFunction|接受一个long类型输入，返回一个double类型结果。|
|30|	LongToIntFunction|接受一个long类型输入，返回一个int类型结果。|
|31|	LongUnaryOperator|接受一个参数同为类型long,返回值类型也为long。|
|32|	ObjDoubleConsumer&lt;T>|接受一个object类型和一个double类型的输入参数，无返回值。|
|33|	ObjIntConsumer&lt;T>|接受一个object类型和一个int类型的输入参数，无返回值。|
|34|	ObjLongConsumer&lt;T>|接受一个object类型和一个long类型的输入参数，无返回值。|
|35|	Predicate&lt;T>|接受一个输入参数，返回一个布尔值结果。|
|36|	Supplier&lt;T>|无参数，返回一个结果。|
|37|	ToDoubleBiFunction&lt;T,U>|接受两个输入参数，返回一个double类型结果|
|38|	ToDoubleFunction&lt;T>|接受一个输入参数，返回一个double类型结果|
|39|	ToIntBiFunction&lt;T,U>|接受两个输入参数，返回一个int类型结果。|
|40|	ToIntFunction&lt;T>|接受一个输入参数，返回一个int类型结果。|
|41|	ToLongBiFunction&lt;T,U>|接受两个输入参数，返回一个long类型结果。|
|42|	ToLongFunction&lt;T>|接受一个输入参数，返回一个long类型结果。|
|43|	UnaryOperator&lt;T>|接受一个参数为类型T,返回值类型也为T。|

## java8中的lambda表达式

　　lambda表达式是java8很重要的一个新特性， 其语法为：
```java
(param1, param2, ...) -> expression

或：

(param1, param2, ...) -> {
    statement1;
    statement2;
    ...
}
```

　　其中，params的类型已经省略了，编译器可以自行判断(如何判断？)。如果没有参数， 只写一个
()即可；如果只有一个参数()可以省略；两个参数及以上， 不可简写。归纳如下：

* 可选类型声明：不需要声明参数类型，编译器可以统一识别参数值。
* 可选的参数圆括号：一个参数无需定义圆括号，但多个参数或无参数需要定义圆括号。
* 可选的大括号：如果主体包含了一个语句，就不需要使用大括号。
* 可选的返回关键字：如果主体只有一个表达式且有返回值，则编译器会自动返回该返回值，
大括号则需要指明要返回的值。

　　lambda表达式示例：
```java
// 1. 不需要参数,返回值为 5  
() -> 5  
  
// 2. 接收一个参数(数字类型),返回其2倍的值  
x -> 2 * x  
  
// 3. 接受2个参数(数字),并返回他们的差值  
(x, y) -> x – y  
  
// 4. 接收2个int型整数,返回他们的和  
(int x, int y) -> x + y  
  
// 5. 接受一个 string 对象,并在控制台打印,不返回任何值(看起来像是返回void)  
(String s) -> System.out.println(s)
```

### lambda表达式的特点

#### 优点
　　Lambda 表达式免去了使用匿名方法的麻烦，
并且给予Java简单但是强大的函数化的编程能力。

#### 变量作用域
* lambda 表达式只能引用标记了 final 的外层局部变量及成员变量，
这就是说不能在 lambda 内部修改定义在域外的变量，否则会编译错误。
* lambda 表达式的局部变量可以不用声明为 final，
但是必须不可被后面的代码修改（即隐性的具有 final 的语义）
* 在 Lambda 表达式当中不允许声明一个与外层局部变量同名的lambda参数或者lambda域内局部变量。 

## java8的方法引用 Method Reference
　　java8的方法引用共分以下四种情况：

### 1. 同一个类的任意对象的方法引用。
 
 　　示例：String::equals. 对应函数式接口：BiPredicate.


```java
@FunctionalInterface
public interface BiPredicate<T, U> {
    /**
     * Evaluates this predicate on the given arguments.
     *
     * @param t the first input argument
     * @param u the second input argument
     * @return {@code true} if the input arguments match the predicate,
     * otherwise {@code false}
     */
    boolean test(T t, U u);
}
```

```java
import java.util.function.BiPredicate;

public class MethodReferenceTest {
    public static void main(String[] args) {
        boolean aEqualsB = equals("aaa", "AAA", String::equals);
        System.out.println(aEqualsB);

        aEqualsB = equals("aaa", "AAA", String::equalsIgnoreCase);
        System.out.println(aEqualsB);
    }

    public static boolean equals(String a, String b, BiPredicate<String, String> predicate) {
       return predicate.test(a, b);
    }
}
```

### 2. 静态方法引用
　　示例： Math::pow. 对应函数式接口：BiFunction
```java
@FunctionalInterface
public interface BiFunction<T, U, R> {
    /**
     * Applies this function to the given arguments.
     *
     * @param t the first function argument
     * @param u the second function argument
     * @return the function result
     */
    R apply(T t, U u);
}
```

```java
import java.util.function.BiFunction;

public class MethodReferenceTest {
    public static void main(String[] args) {
        Double a = 4d;
        Double b = 2d;
        BiFunction<Double, Double, Double> biFunction = Math::pow;
        Double c = pow(a, b, biFunction);
        System.out.println(c);
    }

    public static Double pow(Double a, Double b, BiFunction<Double, Double, Double> biFunction) {
        return biFunction.apply(a, b);
    }
}
```

### 3. 特定实例对象的方法引用
　　示例1： System.out::println. 对应函数式接口：Consumer
```java
@FunctionalInterface
public interface Consumer<T> {
    /**
     * Performs this operation on the given argument.
     *
     * @param t the input argument
     */
    void accept(T t);
}
```

```java
import java.util.function.Consumer;

public class MethodReferenceTest {
    public static void main(String[] args) {
        String s = "Hello World.";
        Consumer<String> consumer = System.out::println;
        print(s, consumer);
    }

    public static void print(String s, Consumer<String> consumer) {
        consumer.accept(s);
    }
}
```

　　注意： 此处的实例对象也可以是this或super.
 
　　示例2： this::equals. 对应的函数式接口：Predicate
```java
@FunctionalInterface
public interface Predicate<T> {
    /**
     * Evaluates this predicate on the given argument.
     *
     * @param t the input argument
     * @return {@code true} if the input argument matches the predicate,
     * otherwise {@code false}
     */
    boolean test(T t);
}    
```

```java
import java.util.function.Predicate;

public class MethodReferenceTest {
    public static void main(String[] args) {
        boolean equals = new MethodReferenceTest().run();
        System.out.println(equals);
    }

    public boolean run() {
        MethodReferenceTest test = new MethodReferenceTest();
        Predicate<MethodReferenceTest> predicate = this::equals;
        return equals(test, predicate);
    }

    public boolean equals(MethodReferenceTest s, Predicate<MethodReferenceTest> predicate) {
        return predicate.test(s);
    }
}
```
### 4. 构造方法调用
　　示例：String::new. 对应的函数式接口：Supplier
```java
@FunctionalInterface
public interface Supplier<T> {
    /**
     * Gets a result.
     *
     * @return a result
     */
    T get();
}
```

```java
import java.util.function.Supplier;

public class MethodReferenceTest {
    public static void main(String[] args) {
        Supplier<String> supplier = String::new;
        String s = supply(supplier);
        System.out.println(s);
    }

    public static String supply(Supplier<String> supplier) {
        return supplier.get();
    }
}
```

总结： 每一个方法引用，其实都可以对应到一个函数式接口， 即：方法引用和lambda表达式是对等的。
如果想使用方法引用，必须按照该方法的特点(几个入参，几个出参)匹配合适的函数式接口(没有的话就
得自己定义)。
