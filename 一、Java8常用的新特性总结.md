# 一、Java8常用的新特性总结

## 1.1.Java8常用特性总览

![image-20190513214122701](http://image.luokangyuan.com/2019-05-13-134130.png)

## 1.2.lambda表达式

在Java8中引入了一个新的操作符“->”,该操作符称为箭头操作符或`Lambda`操作符，左侧：`Lambda`表示式的参数列表，右侧：`Lambda`表达式中所要执行的功能，即把函数作为一个方法的参数，通常多用于匿名内部类的简洁写法，同时`lambda`表达式也是更好的使用`StreamAPI `的基础。

## 1.3.函数式接口

接口中只有一个抽象方法的接口，就叫函数式接口。可以使用注解`@FunctionalInterface`检查是否为函数式接口。例如，我们可以定义一个处理一个数的的函数接口，数就是函数接口的参数，每次怎么处理就是这个函数接口的实现，我们调用这个函数接口的时候就传入要处理的数字和处理逻辑就可。简单示例如下：

```java
@FunctionalInterface
public interface MyFun {
    public Integer getValue(Integer num);
}
```

```java
public Integer operation(Integer num,MyFun mf){
    return mf.getValue(num);
}
```

```java
@Test
public void test5(){
    Integer num = operation(100,(x)-> x*x);
    System.out.println(num);
}
```

### 四大核心函数接口

- `Consumer`消费型接口： 参数类型 T 返回类型 `void` 对类型T的对象应用操作，`Consumer`消费型是传入一个参数，进行处理
- `Supplier`供给型接口： 参数类型 无 返回类型 T 返回类型为T的对象，`Supplier`供给型是得到一些结果
- `Function<T,R>`函数型接口： 参数类型 T 返回类型 R 对了类型为T的对象应用操作，并返回结果，`Function`函数型是传入一个参数，处理后返回一个结果
- `Predicate`断言型接口： 参数类型 T 返回类型` boolean `确定类型为T的对象是否满足某约束，并返回布尔值。`Predicate`断言型就是做一些判断操作

## 1.4.方法引用和数组引用

当要传递给Lambda体的操作，已经有了实现的方法，可以使用方法引用. (实现抽象方法的参数列表，必须与方法引用方法的参数列表保持一致)。 方法引用：使用操作符“::”将方法名和对象或类的名字分割开，例如：

1. 对象::实例方法
2. 类::静态方法
3. 类::实例方法

> 方法引用的实质就是使用更简单的方式代替Lambda表达式

## 1.5.StreamAPI

这个可以说是我用的最多的啦，开发中集合的遍历，分组，过滤，排序，判断，筛选等等，Stream是Java8中处理集合的关键抽象概念，它可以指定你希望对集合进行测操作，可以执行非常复杂的查找，过滤和映射数据的操作，使用Stream API对集合数据进行操作就类似于使用SQL执行的数据库查询查询，Stream API提供了一种高效且易于使用的处理数据的方式。 流（Stream）是数据渠道，用于操作数据源（集合、数组等）所生成的元素序列，“集合讲的是数据，流讲的是计算”，需要注意的是以下三点：

- Stream自己不会存储元素
- Stream不会改变源对象，相会，他们会返回一个持有结果的新的Stream
- Stream操作是延迟执行的，这意味着他们会等到需要结果的时候才执行。

### Stream使用方法

- 创建Stream：一个数据源（集合、数组）获取一个流
- 中间操作：一个中间操作链，对数据源的数据进行处理
- 终止操作：一个终止操作，执行中间操作链，并产生结果。

## 1.6.LocalDateTime

LocalDateTime是一个不可变的日期时间对象，代表日期时间，通常被视为年 - 月 - 日 - 时 - 分 - 秒。这个也是在开发中使用的最多的，例如统计数据的时候根据时间分组后，统计当前季度或者下一季度的，或者获取当前年月，再例如获取当前月的第一天，当前月的最后的一天，下两个月的最后一天等等，使用`LocalDateTime`都可以很简单的完成，举个例子，根据出生日期计算年龄：

```java
DateTimeFormatter dtf = DateTimeFormatter.ISO_DATE;
LocalDate csrq = LocalDate.parse(csrq, dtf);
LocalDate now = LocalDate.now();
return (int)ChronoUnit.YEARS.between(csrq, now);
```

## 1.7.Optional

这个也可以说是一大神器啦，排除空指针异常呀，有时候自己写的你还能注意点盼空，随着微服务的使用，调用其他系统接口，你也不完全知道别人会给你返回个人什么呀，之前我们可以使用三木运算判空或者`if`判空，现在可以使用`Optional`更加优雅的消除空指针。

## 1.8.Base64

这个虽然没有之前的新特性用的多，但是在最近的项目开发中使用带了，就顺便记录一下吧，

```java
String text = "show me the code";

String encoded = Base64.getEncoder().encodeToString(text.getBytes(StandardCharsets.UTF_8));
String decoded = new String(Base64.getDecoder().decode(encoded), StandardCharsets.UTF_8);
```

## 1.9.更多更详细信息

* [Java8新特性学习笔记](http://luokangyuan.com/java8xue-xi-bi-ji/)
* [Java8常用时间操作](http://luokangyuan.com/java8shi-jian-chang-yong-cao-zuo/)

