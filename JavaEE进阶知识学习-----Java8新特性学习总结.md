# lambda表达式

在Java8中引入了一个新的操作符“->”,该操作符称为箭头操作符或Lambda操作符。
左侧：Lambda表示式的参数列表
右侧：Lambda表达式中所要执行的功能

## 语法格式

### 1.无参数，无返回值（）-> System.out.print("Hello Word");

```java
@Test
public void test1(){
    Runnable r = new Runnable() {
        @Override
        public void run() {
            System.out.print("Hello Word");
        }
    };
    r.run();
    System.out.print("===============================");
    Runnable r1 = () -> System.out.print("Hello Word");
    r1.run();
}
```

### 2.一个参数，无返回值（x）-> System.out.print(x);

```java
@Test
public void test2(){
    Consumer<String> con = (x) -> System.out.println(x);
    con.accept("Hello Word");
}
```

如果只有一个参数，无返回值可以省略小括号不写。

### 3.两个参数，有返回值，并且有多条执行语句

```java
@Test
public void test3(){
    Comparator<Integer> com = (x,y) ->{
        System.out.println("函数式接口");
        return Integer.compare(x,y);
    };
    int max = com.compare(4,5);
    System.out.println(max);
}
```

### 4.如果只有一条返回语句

```java
@Test
public void test4(){
    Comparator<Integer> com = (x,y) -> Integer.compare(x,y);
}
```

## 注意说明

**lambda表达式中的参数类型可以省略不写，JVM可以根据上下文推断出类型**

**Lambda表达式需要函数式接口的支持。**

# 函数式接口

接口中只有一个抽象方法的接口，就叫函数式接口。可以使用注解@FunctionalInterface检查是否为函数式接口。

```java
@FunctionalInterface
public interface MyPredicat <T>{
    public boolean test(T t);
}
```

## 函数式接口示例

### 1.定义一个函数式接口

```Java
@FunctionalInterface
public interface MyFun {
    public Integer getValue(Integer num);
}
```

### 2.定义一个方法，方法的参数为函数式接口

```java
public Integer operation(Integer num,MyFun mf){
        return mf.getValue(num);
    }
```

### 3.使用Lambda表达式

```java
@Test
public void test5(){
    Integer num = operation(100,(x)-> x*x);
    System.out.println(num);
}
```

Lambda表达式左侧是函数式接口的参数，右侧是函数式接口的实现。

# Lambda练习

## 练习一

将集合中的员工排序，按照年龄从小到大排序，如果年龄相同就按照名称排序

```java
public class TestLambda {
    List<Employee> emps = Arrays.asList(
            new Employee("张三",13,9999.99),
            new Employee("李四",67,444.44),
            new Employee("王五",45,55.55),
            new Employee("赵六",45,6666.66)
    );
    @Test
    public void test1(){
        Collections.sort(emps,(e1,e2) -> {
            if(e1.getAge() == e2.getAge()){
                return e1.getName().compareTo(e2.getName());
            }else{
                return Integer.compare(e1.getAge(),e2.getAge());
            }
        });
        for(Employee emp:emps){
            System.out.println(emp);
        }
    }
}
```

## 练习二

对字符串进行处理

### 1.申明一个函数式接口，用于处理字符串

```java
@FunctionalInterface
public interface MyFunction {
    public  String getValue(String str);
}
```

### 2.申明一个处理字符串的方法，返回处理后的结果

```java
public String strHandle(String str,MyFunction mf){
    return mf.getValue(str);
}
```

### 3.调用处理方法，使用Lambda表达式实现字符串的不同处理

```java
@Test
public void test2(){
    //将传入的字符串做去除空格处理
    String trimStr = strHandle("  \t\t\t\tHello Word",(str) -> str.trim());
    System.out.println(trimStr);
    //将传入的字符串做大写转换处理
    String uper = strHandle("abce",(str) -> str.toUpperCase());
    System.out.println(uper);
    //将传入的字符串做截取处理
    String subStr = strHandle("我要好好学习，成为一个大神",(str) -> str.substring(1,5));
    System.out.println(subStr);
}
```

## 练习三

计算两个long型参数做处理

### 1.声明一个函数式接口

```java
@FunctionalInterface
public interface MyFunction2 <T,R>{
    public R getValue(T t1,T t2);
}
```

### 2.定义处理方法

```java
public void operator(Long l1,Long l2,MyFunction2<Long,Long> mf){
   System.out.println(mf.getValue(l1,l2));
}
```

### 3.使用Lambda表达式

~~~java
@Test
public void test4(){
    operator(100L,200L,(x,y) -> x+y);
    operator(100L,200L,(x,y) -> x*y);
}	
~~~

# lambda表达式总结

从上述代码中，我们可以看出Lambda表达式的好处，但是我们会发现，每次使用都会新建一个函数式接口，增加了很多麻烦，所以，Java8给我们增加了很多函数式接口，

# 四大核心函数接口

- Consumer<T>消费型接口： 参数类型 T  返回类型 void 对类型T的对象应用操作
- Supplier<T>供给型接口： 参数类型 无 返回类型 T 返回类型为T的对象
- Function<T,R>函数型接口： 参数类型 T 返回类型 R 对了类型为T的对象应用操作，并返回结果
- Predicate<T>断言型接口： 参数类型 T 返回类型 boolean 确定类型为T的对象是否满足某约束，并返回布尔值。

## Consumer示例

```java
@Test
public void test1(){
    happy(10000,(m) -> System.out.println("购买笔记本电脑，每次消费"+m+"元"));
}
public void happy(double money, Consumer<Double> con){
    con.accept(money);
}
```

## Supplier示例

```java
@Test
public void test2(){
   List<Integer> list =  getNumList(10, () -> (int)(Math.random()*100));
   for(Integer num: list){
       System.out.println(num);
   }
}
public List<Integer> getNumList(int num, Supplier<Integer> sup){
    List<Integer> list = new ArrayList<>();
    for (int i = 0; i <num ; i++) {
        Integer n = sup.get();
        list.add(n);
    }
    return list;
}
```

上述使用Lambda表达式就是产生10个100以内的随机数。

## Function示例

```java
@Test
public void test3(){
    String upperStr = strHandle("abce",(str) -> str.toUpperCase());
    System.out.println(upperStr);
}
public String strHandle(String str, Function<String,String> fun){
    return fun.apply(str);//对str进行处理，具体处理方式调用的时候使用Lambda表达式指定
}
```

## Predicate示例

将满足条件的字符串添加到集合中

```java
@Test
public void test4(){
    List<String> list = Arrays.asList("Hello","www.baidu.com","zhangsan","lisi");
    List<String> strList = filterStr(list,(s) -> s.length() > 4);
    for(String str: strList){
        System.out.println(str);
    }
}

public List<String> filterStr(List<String> list , Predicate<String> per){
    List<String> strList = new ArrayList<>();
    for (String str: list) {
        if(per.test(str)){//对str进行过滤操作，具体操作调用的时候才执行
            strList.add(str);
        }
    }
    return strList;
}
```

# 函数接口总结

- Consumer消费型是传入一个参数，进行处理
- Supplier供给型是得到一些结果
- Function函数型是传入一个参数，处理后返回一个结果
- Predicate断言型就是做一些判断操作
- 有无参数和返回值是指Predicate<String> per等调用的方法需不需要参数和有无返回值，例如：per.test(str)、fun.apply(str)、sup.get()等。

# 方法引用

当要传递给Lambda体的操作，已经有了实现的方法，可以使用方法引用.
(实现抽象方法的参数列表，必须与方法引用方法的参数列表保持一致)。
方法引用：使用操作符“::”将方法名和对象或类的名字分割开，例如：

1. 对象::实例方法
2. 类::静态方法
3. 类::实例方法

## 对象::实例方法示例

```java
@Test
public void test1(){
    //注意：con.accept()中的accept的参数类型和返回值和println参数类型和返回值一致
    Consumer<String> com = (x) -> System.out.println(x);
    PrintStream ps = System.out;
    Consumer<String> con = ps::println;
}
```

例如打印一个字符串：

```java
@Test
public void test1(){
    Consumer<String> con = System.out::println;
    con.accept("Hello Word");
}
```

**注意：con.accept()中的accept的参数类型和返回值和println参数类型和返回值一致**

## 类::静态方法

方法引用的实质就是使用更简单的方式代替Lambda表达式。下述代码就是类::静态方法的一个实例。

```java
@Test
public void test2(){
    Comparator<Integer> con = (x,y) -> Integer.compare(x,y);
    //上述代码中Lambda表达体中的compare方法已经被实现，可以简写为
    Comparator<Integer> con1 = Integer::compare;
}
```

## 类::实例方法如下

```java
@Test
public void test3(){
    BiPredicate<String,String> bp = (x,y) -> x.equals(y);
    //上述代码简写为
    BiPredicate<String,String> bp2 = String::equals;
}
```

# 构造器引用

```java
@Test
public void test4(){
    Supplier<Employee> sup = () -> new Employee();
    //构造器引用
    Supplier<Employee> sup2 =  Employee::new;
    Employee employee = sup.get();
    System.out.println(employee.getName());
}
```

其中构造器方法调用哪一个构造器取决与接口Supplier中的方法参数，Supplier就是调用的无参构造器，例如Function函数接口就是传入一个参数，并返回一个结果。如下

```java
@Test
public void test5(){
    Function<String,Employee> fun = (x) -> new Employee(x);
    //构造器引用
    Function<String,Employee> fun2 = Employee::new;
    Employee emp = fun2.apply("王五");
    System.out.println(emp);
}
```

 Function<String,Employee>中的String是传入参数类型，Employee是返回结果类型。
如果我们想传入两个参数，并返回一个结果，就必须要在Employee中创建两个含两个参数的构造器，如下

```java
@Test
public void test6(){
    BiFunction<String,Integer,Employee> fun = Employee::new;
    Employee emp = fun.apply("赵六",123);
    System.out.println(emp);
}
```

**注意：Employee中构造器参数列表和接口中的方法fun.apply("赵六",123);参数列表保持一致。**

# 数组引用

## 数组引用格式

**type[]::new**

```java
@Test
public void test7(){
    Function<Integer,String[]> fun = (x) -> new String[x];
    String[] str= fun.apply(10);
    System.out.println(str.length);
}
```

上述代码就是使用Lambda表达式传入一个数组大小，从而创建一个指定大小和类型的数组。
使用数组引用为：

```java
Function<Integer,String[]> fun2 = String[]::new;
String[] str2 = fun2.apply(10);
System.out.println(str2.length);
```

# StreamAPI

## Stream简介

Stream是Java8中处理集合的关键抽象概念，它可以指定你希望对集合进行测操作，可以执行非常复杂的查找，过滤和映射数据的操作，使用Stream API对集合数据进行操作就类似于使用SQL执行的数据库查询查询，Stream API提供了一种高效且易于使用的处理数据的方式。
流（Stream）是数据渠道，用于操作数据源（集合、数组等）所生成的元素序列，“集合讲的是数据，流讲的是计算”，需要注意的是以下三点

- Stream自己不会存储元素
- Stream不会改变源对象，相会，他们会返回一个持有结果的新的Stream
- Stream操作是延迟执行的，这意味着他们会等到需要结果的时候才执行。

## Stream使用方法

- 创建Stream：一个数据源（集合、数组）获取一个流
- 中间操作：一个中间操作链，对数据源的数据进行处理
- 终止操作：一个终止操作，执行中间操作链，并产生结果。

### 创建Stream的方法

- 通过Collection系列提供的stream()或parallelStream()，如下：
- 通过Arrays中的静态方法stream()方法获取流
- 通过Stream类中的静态方法of()
- 创建无限流

#### 创建Stream的方法示例

```java
@Test
public void test1(){
    // 1.通过Collection系列提供的stream()或parallelStream()
    List<String> list = new ArrayList<>();
    Stream<String> stream = list.stream();

    // 2.通过Arrays中的静态方法stream()方法获取流
    Employee[] emps = new Employee[10];
    Stream<Employee> stream1 = Arrays.stream(emps);

    // 3.通过Stream类中的静态方法of()
    Stream<String> stream2 = Stream.of("AA","BB","CC");

    // 4.创建无限流
    //迭代
    Stream<Integer> stream3 = Stream.iterate(0,(x) -> x+2);
    //只要前10个（中间操作）
    stream3.limit(10).forEach(System.out::println);
    //打印了所有的中间流操作
    //stream3.forEach(System.out::println);
	//4.2生成
    Stream.generate(() -> Math.random())
            .limit(10)
            .forEach(System.out::println);
}
```

### 中间操作

- filter----接受lambda,从流中排除某一些元素
- limit----截断流，使其元素不超过给定的数量
- skip(n)----跳过元素，返回一个扔掉了前n个元素的流，若流中元素不足n个，则返回一个空流
- distinct----筛流，通过流生成元素的hashcode()和equals()去除重复元素

#### filter示例

```java
@Test
public void test1(){
    //中间操作
    Stream<Employee> stream = employees.stream()
            .filter((e) -> {
                System.out.println("中间操作");
                return  e.getAge() > 16;
            });
    //终止操作
    stream.forEach(System.out::println);
}
```

### 惰性求值和内部迭代

1. 如果没有终止操作，是不会打印中间操作的，这就是流只有需要结果的时候才会被调用，这就是惰性求值。
2. 上述打印是Stream自己给我们迭代输出的，这个就是内部迭代。

### 筛选和切片示例如下

```java
@Test
public void test1(){
    //中间操作
    Stream<Employee> stream = employees.stream()
            .filter((e) -> e.getAge() > 15)
            .limit(4)
            .skip(1)
            .distinct();
    //终止操作
    stream.forEach(System.out::println);
}
```

说明：由于distinct是根据hashcode()和equals()去重，所以Employee中要重写equals和hashCode方法。

### 映射

- map(Function f) 接收一个函数作为参数，该函数会被应用到每一个元素上，并将其映射成一个新的元素。
- mapToDouble(ToDoubleFunction f)接收一个函数作为参数，该函数会被应用到每个元素上，产生一个新的DoubleStream
- mapToLong(ToLongFunction f) 接收一个函数作为参数，该函数会被应用到每个元素上，产生一个新的LongStream
- flatMap(Function f) 接收一个函数作为参数，将流中的每个值都换成另一个流，然后把所有流连接成一个流。

#### map示例

```java
@Test
public void test2(){
    List<String> list = Arrays.asList("aaa","bbb","ccc","ddd");
    list.stream()
            .map((str) -> str.toUpperCase())
            .forEach(System.out::println);
System.out.println("================");
    employees.stream()
            .map(Employee::getName)
            .forEach(System.out::println);
}
```

#### flatMap示例

```java
@Test
public void test3(){
    List<String> list = Arrays.asList("aaa","bbb","ccc","ddd");
    Stream<Character> sm = list.stream()
            .flatMap(TestMiddle::filterCharacter);
    sm.forEach(System.out::println);
}

public static  Stream<Character> filterCharacter(String str){
    List<Character> list = new ArrayList<>();
    for(Character ch: str.toCharArray()){
        list.add(ch);
    }
    return list.stream();
}
```

### 排序

#### sorted()-自然排序（comparable）

```java
@Test
    public void test4(){
        List<String> list = Arrays.asList("ccc","aaa","bbb","eee");
        list.stream()
                .sorted()
                .forEach(System.out::println);
    }
}
```

#### sorted(Comparator com)-定制排序（Comparator）

```java
@Test
public void test5(){
    employees.stream()
            .sorted((e1,e2) -> {
                if(e1.getAge().equals(e2.getAge())){
                    return e1.getName().compareTo(e2.getName());
                }else{
                    return e2.getAge().compareTo(e2.getAge());
                }
            }).forEach(System.out::println);
}
```

### 终止操作

终止操作会从流的流水线生成结果，该结果可以是任何不是流的值，例如：List、Integer、void。

#### 查找和匹配

- allMatch(Predicate p) 检查是否匹配所有的元素
- anyMatch(Predicate p) 检查是否至少匹配一个元素
- noneMatch(Predicate p) 检查是否没有匹配所有的元素
- findFirst() 返回第一个元素
- findAny() 返回当前流中的任意元素
- count() 返回流中元素个数
- max(Comparator c) 返回流中最大值
- min(Comparator c) 返回流中最小值
- forEach(Consumer c) 内部迭代

示例如下：

```java
public class TestStreamAPI {
    List<Employee> employees = Arrays.asList(
            new Employee("张三",16,9999.99, Employee.Status.FREE),
            new Employee("李四",18,8888.99, Employee.Status.VOCATION),
            new Employee("王五",20,7777.99, Employee.Status.BUSY),
            new Employee("赵六",22,6666.99, Employee.Status.FREE),
            new Employee("田七",24,5555.99, Employee.Status.BUSY),
            new Employee("小八",26,4444.99, Employee.Status.VOCATION),
            new Employee("陈九",28,3333.99, Employee.Status.VOCATION),
            new Employee("王五",32,9999.99, Employee.Status.BUSY),
            new Employee("王五",34,9999.99, Employee.Status.FREE),
            new Employee("王五",36,9999.99, Employee.Status.BUSY)
    );
    @Test
    public void test1(){
       //检查是否所有的状态为BUSY状态
       Boolean b1 =  employees.stream()
                .allMatch((e) -> e.getStatus().equals(Employee.Status.BUSY));
       //检查是否至少匹配一个元素
       Boolean b2 = employees.stream()
               .anyMatch((e) -> e.getStatus().equals(Employee.Status.BUSY));
       //检查是否没有匹配元素
       Boolean b3 = employees.stream()
                .noneMatch((e) -> e.getStatus().equals(Employee.Status.BUSY));
       //先按照工资排序，再去除第一个元素放在Option容器中
       Optional<Employee> op = employees.stream()
               .sorted((e1,e2) -> Double.compare(e1.getSalay(),e2.getSalay()))
               .findFirst();
       System.out.println(op.get());
       //findAny返回当前流中的任意元素，先过滤再返回一个，
       //mployees.stream()实现的是串行流，每次返回的值一定的。
       //employees.parallelStream()实现的是并行流 ，返回的就是满足条件的随机结果。
       Optional<Employee> op2 = employees.parallelStream()
               .filter((e) -> e.getStatus().equals(Employee.Status.BUSY))
               .findAny();
       System.out.println(op2.get());
       //返回流中元素个数
       Long count = employees.stream()
                .count();
       //返回流中最大值
       Optional<Employee> op1 = employees.stream()
               .max((e1,e2) -> Double.compare(e1.getSalay(),e2.getSalay()));
       System.out.println(op1.get());
       //返回流中最低工资
       Optional<Double> op3 = employees.stream()
               .map(Employee::getSalay)
               .min(Double::compare);
       System.out.println(op3.get());
    }
}
```

### 归约

reduce(T iden,BinaryOperator b)可以将流中元素反复结合起来，得到一个值，返回T,示例如下：

```java
@Test
public void test2(){
    List<Integer> list = Arrays.asList(1,2,3,4,5,6,7,8,9,10);
    Integer sum = list.stream()
            .reduce(0,(x,y) -> x+y);
    System.out.println(sum);
    //计算工资的总和
    Optional<Double> op = employees.stream()
            .map(Employee::getSalay)
            .reduce(Double::sum);
    System.out.println(op.get());
}
```

上述代码中使用map得到所有的工资，再有reduce将所有的工资累加，map和reduce配合使用情况比较多。

### 收集

collect(Collector c)将流转换为其他形式，接收一个Collector接口的实现，用于给Stream中元素做汇总的方法。
Colletor接口中的方法实现决定了如何对流执行收集操作（如收集到List，Set,Map）,但是Collectors实用类提供了很多静态的方法，可以方便的创建常用收集器实例，具体方法与实例如下：

- toList 返回List<T> 将流中元素搜集到List
- toSet 返回Set<T> 将流中元素手机到Set
- toCollection 返回Collection<T> 把流中元素收集到创建的集合中
- counting 返回Long 计算流中元素的个数
- summinglnt 返回Integer 对流中元素的整数属性求和
- averaginglnt 返回Double 计算流中元素Integer属性的平均值
- summarizinglnt 返回IntSummaryStatistics 计算流中Integer属性的统计值，如平均值。
- joining 返回String 连接流中每一个字符串
- maxBy 返回Optional<T> 根据比较器选择最大值
- minBy 返回Optional<T> 根据比较器选择最小值
- reducing 归约产生的类型
- collectionAndThen 转换函数返回的类型

实例如下：

```java
@Test
public void test3(){
    //将名字添加到list中
    List<String> list = employees.stream()
            .map(Employee::getName)
            .collect(Collectors.toList());
    list.forEach(System.out::println);
    //将名字添加到Set中
    Set<String> set = employees.stream()
            .map(Employee::getName)
            .collect(Collectors.toSet());
    set.forEach(System.out::println);
    //将名字添加到特定的数据结构中
    HashSet<String> hashSet = employees.stream()
            .map(Employee::getName)
            .collect(Collectors.toCollection(HashSet::new));
    //总数
    Long count = employees.stream()
            .collect(Collectors.counting());
    //平均数
    Double avg = employees.stream()
            .collect(Collectors.averagingDouble(Employee::getSalay));
    //总和
    Double sum = employees.stream()
            .collect(Collectors.summingDouble(Employee::getSalay));
    //最大值，返回工资最大的员工信息
    Optional<Employee> max = employees.stream()
            .collect(Collectors.maxBy((e1,e2) -> Double.compare(e1.getSalay(),e2.getSalay())));
    //最小值，返回最小工资
    Optional<Double> min = employees.stream()
            .map(Employee::getSalay)
            .collect(Collectors.minBy(Double::compare));
    //按照状态分组
    Map<Employee.Status,List<Employee>> map = employees.stream()
            .collect(Collectors.groupingBy(Employee::getStatus));
    map.get(Employee.Status.BUSY);
    //多级分组
    Map<Employee.Status,Map<String,List<Employee>>> map1 = employees.stream()
           .collect(Collectors.groupingBy(Employee::getStatus,Collectors.groupingBy((e) ->{
               if(((Employee)e).getAge() <= 18){
                   return "少年";
               }else if(((Employee)e).getAge() <= 26){
                   return "中年";
               }else{
                   return "老年";
               }
           })));
    System.out.println(map1);
    //分区。满足条件一个区，不满足条件的一个区
    Map<Boolean,List<Employee>> map2 = employees.stream()
            .collect(Collectors.partitioningBy((e) -> e.getSalay() >5000));
    System.out.println(map2);
    //其他的一种获取方式
    DoubleSummaryStatistics dss = employees.stream()
            .collect(Collectors.summarizingDouble(Employee::getSalay));
    System.out.println(dss.getSum());
    System.out.println(dss.getAverage());
    System.out.println(dss.getMax());
    //连接字符串
    String str = employees.stream()
            .map(Employee::getName)
            .collect(Collectors.joining(",","====","==="));
}
```

# StreamAPI练习

## 1.给定一个数字列表，返回一个由每一个数的平方构成的列表。

```java
@Test
public void test(){
    Integer[] nums = new Integer[]{1,2,3,4,5};
    Arrays.stream(nums)
            .map((x) -> x*x)
            .forEach(System.out::println);

}
```

## 2.使用map和reduce方法数一数流中有多少个Employee

```java
@Test
public void test1(){
  Optional<Integer> count = employees.stream()
           .map((e) -> 1)
           .reduce(Integer::sum);
  System.out.println(count.get());
}
```

## 交易员类

```java
public class Trader {

	private String name;
	private String city;

	public Trader() {
	}

	public Trader(String name, String city) {
		this.name = name;
		this.city = city;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public String getCity() {
		return city;
	}

	public void setCity(String city) {
		this.city = city;
	}

	@Override
	public String toString() {
		return "Trader [name=" + name + ", city=" + city + "]";
	}

}
```

## 交易类

```java
public class Transaction {

	private Trader trader;
	private int year;
	private int value;

	public Transaction() {
	}

	public Transaction(Trader trader, int year, int value) {
		this.trader = trader;
		this.year = year;
		this.value = value;
	}

	public Trader getTrader() {
		return trader;
	}

	public void setTrader(Trader trader) {
		this.trader = trader;
	}

	public int getYear() {
		return year;
	}

	public void setYear(int year) {
		this.year = year;
	}

	public int getValue() {
		return value;
	}

	public void setValue(int value) {
		this.value = value;
	}

	@Override
	public String toString() {
		return "Transaction [trader=" + trader + ", year=" + year + ", value="
				+ value + "]";
	}
}
```

## 练习

```java
public class TestTransaction {
	
	List<Transaction> transactions = null;
	
	@Before
	public void before(){
		Trader raoul = new Trader("Raoul", "Cambridge");
		Trader mario = new Trader("Mario", "Milan");
		Trader alan = new Trader("Alan", "Cambridge");
		Trader brian = new Trader("Brian", "Cambridge");
		
		transactions = Arrays.asList(
				new Transaction(brian, 2011, 300),
				new Transaction(raoul, 2012, 1000),
				new Transaction(raoul, 2011, 400),
				new Transaction(mario, 2012, 710),
				new Transaction(mario, 2012, 700),
				new Transaction(alan, 2012, 950)
		);
	}
	
	//1. 找出2011年发生的所有交易， 并按交易额排序（从低到高）
	@Test
	public void test1(){
		transactions.stream()
				.filter((t) -> t.getYear() == 2011)//过滤2011年的交易
				.sorted((t1,t2) -> Integer.compare(t1.getValue(),t2.getValue()))
				.forEach(System.out::println);
	}
	//2. 交易员都在哪些不同的城市工作过？
	@Test
	public void test2(){
		transactions.stream()
				.map((t) -> t.getTrader().getCity())
				.distinct()
				.forEach(System.out::println);
	}
	//3. 查找所有来自剑桥的交易员，并按姓名排序
	@Test
	public void test3(){
		new ArrayList<Integer>();
		transactions.stream()
				.filter((t) -> t.getTrader().getCity().equals("Cambridge"))
				.map(Transaction::getTrader)
				.sorted((t1,t2) -> t1.getName().compareTo(t2.getName()))
				.distinct()
				.forEach(System.out::println);
	}
	//4. 返回所有交易员的姓名字符串，按字母顺序排序
	@Test
	public void test4(){
		transactions.stream()
				.map((t) -> t.getTrader().getName())
				.sorted()
				.forEach(System.out::println);
		System.out.println("====================================");
		String str = transactions.stream()
				.map((t) -> t.getTrader().getName())
				.sorted()
				.reduce("",String::concat);
		System.out.println(str);
	}
	//5. 有没有交易员是在米兰工作的？
	@Test
	public void test5(){
		Boolean b1 = transactions.stream()
				.anyMatch((t) -> t.getTrader().getCity().equals("Milan"));
		System.out.println(b1);
	}
	//6. 打印生活在剑桥的交易员的所有交易额
	@Test
	public void test6(){
		Optional<Integer> sum = transactions.stream()
				.filter((e) -> e.getTrader().getCity().equals("Cambridge"))
				.map(Transaction::getValue)
				.reduce(Integer::sum);
		System.out.println(sum.get());
	}
	//7. 所有交易中，最高的交易额是多少
	@Test
	public void test7(){
		Optional<Integer> max = transactions.stream()
				.map((t) -> t.getValue())
				.max(Integer::compare);
		System.out.println(max);
	}
	//8. 找到交易额最小的交易
	@Test
	public void test8(){
		Optional<Transaction> op = transactions.stream()
				.min((t1,t2) -> Integer.compare(t1.getValue(),t2.getValue()));
		System.out.println(op.get());
	}
}
```

# LocalDateTime

LocalDateTime是一个不可变的日期时间对象，代表日期时间，通常被视为年 - 月 - 日 - 时 - 分 - 秒。

## 方法摘要

```properties
Temporal adjustInto(Temporal temporal) 调整指定的时间对象与此对象具有相同的日期和时间。  
OffsetDateTime atOffset(ZoneOffset offset) 将此日期时间与偏移量相结合以创建 OffsetDateTime 。  
ZonedDateTime atZone(ZoneId zone) 将此日期时间与时区相结合以创建 ZonedDateTime 。  
int compareTo(ChronoLocalDateTime<?> other) 将此日期时间与其他日期时间进行比较。  
boolean equals(Object obj) 检查这个日期时间是否等于另一个日期时间。  
String format(DateTimeFormatter formatter) 使用指定的格式化程序格式化此日期时间。  
static LocalDateTime from(TemporalAccessor temporal) 从时间对象获取一个 LocalDateTime的实例。  
int get(TemporalField field) 从此日期时间获取指定字段的值为 int 。  
int getDayOfMonth() 获取月份字段。  
DayOfWeek getDayOfWeek() 获取星期几字段，这是一个枚举 DayOfWeek 。  
int getDayOfYear() 获得日期字段。  
int getHour() 获取时间字段。  
long getLong(TemporalField field) 从此日期时间获取指定字段的值为 long 。  
int getMinute() 获取小时字段。  
Month getMonth() 使用 Month枚举获取月份字段。  
int getMonthValue() 将月份字段从1到12。  
int getNano() 获得纳秒第二场。  
int getSecond() 获得第二分钟的字段。  
int getYear() 获取年份字段。  
int hashCode() 这个日期时间的哈希码。  
boolean isAfter(ChronoLocalDateTime<?> other) 检查这个日期时间是否在指定的日期之后。  
boolean isBefore(ChronoLocalDateTime<?> other) 检查此日期时间是否在指定的日期时间之前。  
boolean isEqual(ChronoLocalDateTime<?> other) 检查此日期时间是否等于指定的日期时间。  
boolean isSupported(TemporalField field) 检查指定的字段是否受支持。  
boolean isSupported(TemporalUnit unit) 检查指定的单位是否受支持。  
LocalDateTime minus(long amountToSubtract, TemporalUnit unit) 返回此日期时间的副本，并减去指定的金额。  
LocalDateTime minus(TemporalAmount amountToSubtract) 返回此日期时间的副本，并减去指定的金额。  
LocalDateTime minusDays(long days) 返回此 LocalDateTime的副本，其中指定的时间间隔以天为单位。  
LocalDateTime minusHours(long hours) 以指定的时间段返回此 LocalDateTime的副本，以减少的小时数。  
LocalDateTime minusMinutes(long minutes) 返回此 LocalDateTime的副本，以指定的时间间隔减去。  
LocalDateTime minusMonths(long months) 返回此 LocalDateTime的副本，指定的时间以月为单位减去。  
LocalDateTime minusNanos(long nanos) 返回这个 LocalDateTime的副本，以指定的时间减去纳秒。  
LocalDateTime minusSeconds(long seconds) 返回此 LocalDateTime的副本，其中指定的时间间隔以秒为单位。  
LocalDateTime minusWeeks(long weeks) 返回此 LocalDateTime的副本，其中指定的周期以周为单位减去。  
LocalDateTime minusYears(long years) 返回此 LocalDateTime的副本，并以减去的年份为单位。  
static LocalDateTime now() 从默认时区的系统时钟获取当前的日期时间。  
static LocalDateTime now(Clock clock) 从指定的时钟获取当前的日期时间。  
static LocalDateTime now(ZoneId zone) 从指定时区的系统时钟获取当前的日期时间。
static LocalDateTime of(int year, int month, int dayOfMonth, int hour, int minute) 
从年，月，日，小时和分钟获得LocalDateTime的实例，将第二和纳秒设置为零。
static LocalDateTime of(int year, int month, int dayOfMonth, int hour, int minute, int second) 
从年，月，日，小时，分钟和秒获得 LocalDateTime的实例，将纳秒设置为零。  
static LocalDateTime of(int year, int month, int dayOfMonth, int hour, int minute, int second, int nanoOfSecond)
 获取的实例 LocalDateTime从年，月，日，小时，分钟，秒和纳秒。  
static LocalDateTime of(int year, Month month, int dayOfMonth, int hour, int minute) 
从年，月，日，小时和分钟获得 LocalDateTime的实例，将第二和纳秒设置为零。  
static LocalDateTime of(int year, Month month, int dayOfMonth, int hour, int minute, int second) 
从年，月，日，小时，分钟和秒获得 LocalDateTime的实例，将纳秒设置为零。  
static LocalDateTime of(int year, Month month, int dayOfMonth, int hour, int minute, int second, int nanoOfSecond)
获取的实例 LocalDateTime从年，月，日，小时，分钟，秒和纳秒。  
static LocalDateTime of(LocalDate date, LocalTime time) 
从日期和时间获取 LocalDateTime一个实例。  
static LocalDateTime ofEpochSecond(long epochSecond, int nanoOfSecond, ZoneOffset offset) 
使用从1970-01-01T00：00：00Z的时代开始的秒数获得一个 LocalDateTime的实例。  
static LocalDateTime ofInstant(Instant instant, ZoneId zone) 
从 Instant和区域ID获取一个 LocalDateTime的实例。  
static LocalDateTime parse(CharSequence text) 
从一个文本字符串（如 2007-12-03T10:15:30获取一个 LocalDateTime的实例。  
static LocalDateTime parse(CharSequence text, DateTimeFormatter formatter)
 使用特定的格式化 LocalDateTime从文本字符串获取 LocalDateTime的实例。  
LocalDateTime plus(long amountToAdd, TemporalUnit unit) 返回此日期时间的副本，并添加指定的金额。  
LocalDateTime plus(TemporalAmount amountToAdd) 返回此日期时间的副本，并添加指定的金额。  
LocalDateTime plusDays(long days) 返回此 LocalDateTime的副本，并以指定的时间段添加天数。  
LocalDateTime plusHours(long hours) 以指定的时间（以小时为单位）返回此 LocalDateTime的副本。  
LocalDateTime plusMinutes(long minutes) 以指定的时间（以分钟为单位）返回此 LocalDateTime的副本。  
LocalDateTime plusMonths(long months) 返回这个 LocalDateTime的副本，其中指定的时间段以月为单位。  
LocalDateTime plusNanos(long nanos) 返回这个 LocalDateTime的副本，其指定时间以纳秒为单位。  
LocalDateTime plusSeconds(long seconds) 以指定的时间段返回此 LocalDateTime的副本，以秒为单位。  
LocalDateTime plusWeeks(long weeks) 返回这个 LocalDateTime的副本，并以指定的周期添加周数。  
LocalDateTime plusYears(long years) 返回这个 LocalDateTime的副本，其中指定的时间段以添加的年数表示。  
<R> R query(TemporalQuery<R> query) 使用指定的查询查询此日期时间。  
ValueRange range(TemporalField field) 获取指定字段的有效值的范围。  
LocalDate toLocalDate() 获得这个日期时间的 LocalDate一部分。  
LocalTime toLocalTime() 获得这个日期时间的 LocalTime一部分。  
String toString() 将此日期时间输出为 String ，例如 2007-12-03T10:15:30 。  
LocalDateTime truncatedTo(TemporalUnit unit) 返回此 LocalDateTime的副本， LocalDateTime时间。  
long until(Temporal endExclusive, TemporalUnit unit) 根据指定的单位计算到另一个日期时间的时间量。  
LocalDateTime with(TemporalAdjuster adjuster) 返回此日期时间的调整副本。  
LocalDateTime with(TemporalField field, long newValue) 返回此日期时间的副本，并将指定的字段设置为新值。  
LocalDateTime withDayOfMonth(int dayOfMonth) 返回此 LocalDateTime的副本。  
LocalDateTime withDayOfYear(int dayOfYear) 返回这个 LocalDateTime的副本，并更改日期。  
LocalDateTime withHour(int hour) 返回此日期值更改的 LocalDateTime的副本。  
LocalDateTime withMinute(int minute) 返回这个 LocalDateTime的副本，小时值更改。  
LocalDateTime withMonth(int month) 返回此年份更改的 LocalDateTime的副本。  
LocalDateTime withNano(int nanoOfSecond) 返回这个 LocalDateTime的副本，纳秒变化值。  
LocalDateTime withSecond(int second) 返回这个 LocalDateTime的副本，其中 LocalDateTime了第二分钟的值。  
LocalDateTime withYear(int year) 返回这个 LocalDateTime的副本，年份被更改。  
```

