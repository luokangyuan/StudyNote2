# 一、apache.Collections-Map学习笔记

在`Java Collections API`中，我们经常会使用到`list`、`map`、`Collection`、`Arrays`、`Collections`等等。今天，我们就来学习总结一下`org.apache.commons.collections `的常用类。

* `org.apache.commons.collections `– Commons Collections自定义的一组公用的接口和工具类
* `org.apache.commons.collections.bag` – 实现Bag接口的一组类
* `org.apache.commons.collections.bidimap` – 实现BidiMap系列接口的一组类
* `org.apache.commons.collections.buffer` – 实现Buffer接口的一组类
* `org.apache.commons.collections.collection` – 实现java.util.Collection接口的一组类
* `org.apache.commons.collections.comparators` – 实现java.util.Comparator接口的一组类
* `org.apache.commons.collections.functors `– Commons Collections自定义的一组功能类
* `org.apache.commons.collections.iterators` – 实现java.util.Iterator接口的一组类
* `org.apache.commons.collections.keyvalue` – 实现集合和键/值映射相关的一组类
* `org.apache.commons.collections.list` – 实现java.util.List接口的一组类
* `org.apache.commons.collections.map` – 实现Map系列接口的一组类
* `org.apache.commons.collections.set` – 实现Set系列接口的一组类

# 二、collections.map常用类

## 2.1.双向Map-BidiMap

双向Map，可以通过key找到value，也可以通过value找到key，需要注意的是**BidiMap当中不光key不能重复，value也不可以重复。**

```java
BidiMap bidiMap = new DualHashBidiMap();
bidiMap.put("name","zhangsan");
bidiMap.put("age",34);
System.out.println(bidiMap.get("name"));
System.out.println(bidiMap.getKey("zhangsan"));
```

## 2.2.一对多Map-MultiMap

无论是在`HashMap`中，还是刚说到的`BidiMap`中，都是简单的一个`key`对应一个`value`，`MultiMap`就是说一个`key`不在是简单的指向一个对象，而是一组对象，add()和remove()的时候跟普通的`Map`无异，只是在`get()`时返回一个`Collection`，利用`MultiMap`，我们就可以很方便的往一个key上放数量不定的对象，也就实现了一对多。

### MultiMap示例说明

当我们需要存放一个小学不同年级和人的时候，我相信大家以前肯定都会想到用年级作`key`，用`List<Student>`作`value`，现在我们使用`MultiMap`接口中的一个`ArrayListMultimap`实现类来说明。

### 导入ArrayListMultimap的Maven坐标

```xml
<dependency>
    <groupId>com.google.guava</groupId>
    <artifactId>guava</artifactId>
    <version>27.0-jre</version>
</dependency>
```

### 编写学生实体类

```java
public class Student{
    String name;
    int age;
}
```

### 编写测试类

```java
public class MultimapTest {
    private static final String CLASS_NAME_1 = "小学五年级";
    private static final String CLASS_NAME_2 = "小学六年级";
    Multimap<String, Student> multimap = ArrayListMultimap.create();

    public void testStudent() {
        for (int i = 0; i < 5; i++) {
            Student student = new Student();
            student.name = "小学五年级学生，学号为：5000" + i;
            student.age = 6;
            multimap.put(CLASS_NAME_1, student);
        }
        for (int i = 0; i < 5; i++) {
            Student student = new Student();
            student.name = "小学六年级学生，学号为：6000" + i;
            student.age = 7;
            multimap.put(CLASS_NAME_2, student);
        }
        System.out.println(multimap);
        for (Student stu : multimap.get(CLASS_NAME_1)) {
            System.out.println("小学五年级学生 name:" + stu.name + " age:" + stu.age);
        }
    }
}
```

### 测试代码

```java
public static void main(String[] args) {
    MultimapTest multimapTest = new MultimapTest();
    multimapTest.testStudent();
}
```

### 测试结果

```properties
小学五年级学生 name:小学五年级学生，学号为：50000 age:6
小学五年级学生 name:小学五年级学生，学号为：50001 age:6
小学五年级学生 name:小学五年级学生，学号为：50002 age:6
小学五年级学生 name:小学五年级学生，学号为：50003 age:6
小学五年级学生 name:小学五年级学生，学号为：50004 age:6
```

其实，这里我们可以输出multimap，可以看到如下信息：

```properties
{
小学五年级学生=[com.luo.commontsdemo.bean.MultimapTest$Student@4ec6a292, ...], 
小学六年级学生=[com.luo.commontsdemo.bean.MultimapTest$Student@5a01ccaa, ...]
}
```

当然，还有很多其他常用方法

```java
//判断键是否存在
if(multimap.containsKey(CLASS_NAME_1)){
    System.out.println("键值包含："+CLASS_NAME_1);
}
//”键-单个值映射”的个数
System.out.println(multimap.size());
//不同键的个数
System.out.print(multimap.keySet().size());
```

> 注意：**不会有任何键映射到空集合：一个键要么至少到一个值，要么根本就不在Multimap中**；
>
> **Multimap.get(key)总是返回非null、但是可能空的集合**

## 2.3.新集合类型Multiset

Guava提供了一个新集合类型**Multiset**，**它可以多次添加相等的元素，且和元素顺序无关**。Multiset继承于JDK的Cllection接口，而不是Set接口。

```java
Multiset<String> hashMultiset = HashMultiset.create();
hashMultiset.add("zhangsan");
hashMultiset.add("zhangsan");
for (String key : hashMultiset.elementSet()) {
    System.out.println(key + "-->" + hashMultiset.count(key));
}
```

### Multiset常用方法

- add(E element) :向其中添加单个元素
- add(E element,int occurrences) : 向其中添加指定个数的元素
- count(Object element) : 返回给定参数元素的个数
- remove(E element) : 移除一个元素，其count值 会响应减少
- remove(E element,int occurrences): 移除相应个数的元素
- elementSet() : 将不同的元素放入一个Set中
- entrySet(): 类似与Map.entrySet 返回Set<Multiset.Entry>。包含的Entry支持使用getElement()和getCount()
- setCount(E element ,int count): 设定某一个元素的重复次数
- setCount(E element,int oldCount,int newCount): 将符合原有重复个数的元素修改为新的重复次数
- retainAll(Collection c) : 保留出现在给定集合参数的所有的元素
- removeAll(Collectionc) : 去除出现给给定集合参数的所有的元素

```java
//向集合中添加若干个元素
wordMultiset.add("and", 10);
for (String key : wordMultiset.elementSet()) {
    System.out.println(key + "-->" + wordMultiset.count(key));
}
```

```java
wordMultiset.remove("that");
for (String key : wordMultiset.elementSet()) {
    System.out.println(key + "-->" + wordMultiset.count(key));
}
```

