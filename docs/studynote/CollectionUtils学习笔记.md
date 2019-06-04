<p align="center">
<img width="130" align="center" src="http://image.luokangyuan.com/Java.svg"/>
</p>
<h1 align="center">CollectionUtils学习笔记</h1>

## 1.1.CollectionUtils常用方法

### 集合判空

`isEmpty(Collection coll)`判断集合是否为空，当然，`isNotEmpty(Collection coll)`判断集合不为空。

```java
String[] arr = {"H", "E", "L"};
List<String> list = Arrays.asList(arr);
CollectionUtils.isEmpty(list);
```

### 数组或集合求并集

使用`union(final Collection a, final Collection b)`方法。

```java
String[] arr1 = {"H", "E", "L"};
String[] arr2 = {"H", "W", "L"};
List<String> list1 = Arrays.asList(arr1);
List<String> list2 = Arrays.asList(arr2);
Collection union = CollectionUtils.union(list1, list2);
/* 结果为：[E, W, H, L]*/
System.out.println(ArrayUtils.toString(union));
```

### 数组或集合求交集

使用`intersection(final Collection a, final Collection b)`方法。

```java
Collection intersection = CollectionUtils.intersection(list1, list2);
/* 结果为：[H, L]*/
System.out.println(ArrayUtils.toString(intersection));
```

### 数组或集合交集的补集

使用`disjunction(final Collection a, final Collection b)`方法。

```java
Collection disjunction = CollectionUtils.disjunction(list1, list2);
/* 结果为：[E, W]*/
System.out.println(ArrayUtils.toString(disjunction));
```

### 数组或集合交集的差集

使用`subtract(final Collection a, final Collection b)`方法，返回的是a对于b的差集。

```java
Collection subtract = CollectionUtils.subtract(list1, list2);
/*结果为：[W]*/
System.out.println(ArrayUtils.toString(subtract));
```

### 判断是否为子集

使用`isSubCollection(final Collection a, final Collection b)`方法，判断集合a是否是集合b的子集；

```java
boolean subCollection = CollectionUtils.isSubCollection(list2, list1); 
```

### 判断两个集合是否相等

使用`isEqualCollection(final Collection a, final Collection b)`方法。

```java
boolean equalCollection = CollectionUtils.isEqualCollection(list1, list2);
```

### 过滤集合中满足条件的数据

使用`CollectionUtils.filter`方法。

```java
List<Integer> list = new ArrayList<>();
list.add(4);
list.add(5);
list.add(6);
list.add(7);
CollectionUtils.filter(list,item ->{
    return Integer.valueOf(item.toString()) > 5;
});
System.out.println(list); = [6, 7]
```

### 返回集合中满足条件的元素数量

使用`CollectionUtils.countMatches`方法。

```java
int count = CollectionUtils.countMatches(list,item -> {
    return Integer.valueOf(item.toString()) > 6;
});
System.out.println(count); = 1
```

### 判断集合中是否存在满足条件的元素

使用`CollectionUtils.exists`方法。

```java
boolean exists = CollectionUtils.exists(list, item -> {
    return Integer.valueOf(item.toString()) > 6;
});
System.out.println(exists); = true
```

### 从集合中查找满足条件的第一个元素

使用`CollectionUtils.find`方法。

```java
Object o = CollectionUtils.find(list, item -> {
    return Integer.valueOf(item.toString()) > 2;
});
System.out.println(o.toString()); = 6
```

### 返回符合条件的Collection

使用`CollectionUtils.select`方法。

```java
Collection select = CollectionUtils.select(list, item -> {
    return Integer.valueOf(item.toString()) > 2;
});
```

### 返回不符合条件的Collection

使用`CollectionUtils.selectRejected`方法，与`CollectionUtils.select`相反。

```java
Collection select = CollectionUtils.selectRejected(list, item -> {
    return Integer.valueOf(item.toString()) > 2;
});
```
