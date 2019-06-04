<p align="center">
<img width="130" align="center" src="http://image.luokangyuan.com/Java.svg"/>
</p>
<h1 align="center">MultiSet学习笔记</h1>

首先，我们来假定一个场景，统计一个词在文档中出现了多少次，传统的做法是这样的：。

我们可以这样来实现,

```java
Map<String, Integer> counts = new HashMap<String, Integer>();
for (String word : words) {
    Integer count = counts.get(word);
    if (count == null) {
        counts.put(word, 1);
    } else {
        counts.put(word, count + 1);
    }
}
```

## 1.1.什么是MultiSet

Guava提供了一个新集合类型 Multiset，它可以多次添加相等的元素，我们可以这样来看待MultiSet

* `没有元素顺序限制的ArrayList<E>`,也就是说`Multiset {a, a, b}`和`{a, b, a}`是相等的。
* `Map<E, Integer>`，键为元素，值为计数。

### 没有元素顺序限制的`ArrayList<E>`

当把Multiset看成普通的Collection时，它表现得就像无序的ArrayList

* add(E)添加单个给定元素
* iterator()返回一个迭代器，包含Multiset的所有元素（包括重复的元素）
* size()返回所有元素的总个数（包括重复的元素）

```java
public void testHashMultiset(){
    HashMultiset<String> hashMultiset = HashMultiset.create();
    hashMultiset.add("zhangsan");
    hashMultiset.add("lisi");
    System.out.println(hashMultiset.size());
    Iterator<String> iterator = hashMultiset.iterator();
    while (iterator.hasNext()) {
        System.out.println(iterator.next());
    }
}
```

### 当把`Multiset`看作`Map<E, Integer>`时，它也提供了符合性能期望的查询操作：

* `count(Object)`返回给定元素的计数。`HashMultiset.count`的复杂度为`O(1)`，`TreeMultiset.count`的复杂度为`O(log n)`。
* `entrySet()`返回`Set<Multiset.Entry<E>>`，和Map的entrySet类似。
* `elementSet()`返回所有不重复元素的`Set<E>`，和Map的keySet()类似。
* 所有`Multiset`实现的内存消耗随着不重复元素的个数线性增长。

```java
public void testHashMultiset(){
    HashMultiset<String> hashMultiset = HashMultiset.create();
    hashMultiset.add("zhangsan",12);
    hashMultiset.add("lisi",5);
    System.out.println(hashMultiset.count("lisi"));
    Set<Entry<String>> entrySet = hashMultiset.entrySet();
    for (Entry<String> entry : entrySet) {
        System.out.println(entry.getElement() + "===" + entry.getCount());
    }
    Set<String> elementSet = hashMultiset.elementSet();
    for (String element : elementSet) {
        System.out.println(element);
    }  
}
```

> 注意，Multiset.addAll(Collection)可以添加Collection中的所有元素并进行计数，这比用for循环往Map添加元素和计数方便多了。

### Multiset常用方法

| 方法             | 描述                                                         |
| ---------------- | ------------------------------------------------------------ |
| count(E)         | 给定元素在Multiset中的计数                                   |
| elementSet()     | Multiset中不重复元素的集合，类型为Set`<E>`                   |
| entrySet()       | 和Map的entrySet类似，返回Set`<Multiset.Entry<E>>`，其中包含的Entry支持getElement()和getCount()方法 |
| add(E, int)      | 增加给定元素在Multiset中的计数                               |
| remove(E, int)   | 减少给定元素在Multiset中的计数                               |
| setCount(E, int) | 设置给定元素在Multiset中的计数，不可以为负数                 |
| size()           | 返回集合元素的总个数（包括重复的元素）                       |

## 1.2.Multiset的各种实现

| Map               | 对应的Multiset         | 是否支持null元素 |
| ----------------- | ---------------------- | ---------------- |
| HashMap           | HashMultiset           | 是               |
| TreeMap           | TreeMultiset           | 是               |
| LinkedHashMap     | LinkedHashMultiset     | 是               |
| ConcurrentHashMap | ConcurrentHashMultiset | 否               |
| ImmutableMap      | ImmutableMultiset      | 否               |
