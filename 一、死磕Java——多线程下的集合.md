# 一、死磕Java——多线程下的集合

## 1.1.ArrayList

都知道`ArrayList`是线程不安全的，如果在多线程下使用了`ArrayList`	会产生什么样的情况，简单看一段代码。

```java
public static void main(String[] args) {
    List<String> list = new ArrayList<>();

    for (int i = 0; i < 3; i++) {
        new Thread(() -> {
            list.add(UUID.randomUUID().toString().substring(0, 5));
            System.out.println(list);
        }, String.valueOf(i)).start();
    }
}
```

代码很简单，就是三个线程向`List`集合中添加数据，多次运行不仅结果确多种多样，还会出现报错。

```properties
[8b496, 686bd, a5bad]
[8b496, 686bd, a5bad]
[8b496, 686bd, a5bad]
```

```properties
[null, c7fe4]
[null, c7fe4]
[null, c7fe4]
```

```properties
[null, 17e5f, 4efd9]
[null, 17e5f, 4efd9]
[null, 17e5f, 4efd9]
```

```properties
[null, null, 5b8ad]
[null, null, 5b8ad]
[null, null, 5b8ad]
```

![image-20190504131426678](http://image.luokangyuan.com/2019-05-04-051431.png)

通过查看`ArrayList`的`add`方法源码就可以看到并没有加锁，所以在多线程环境下肯定会出现线程不安全问题，`ArrayList`的`add`方法源码如下：

```java
public boolean add(E e) {
    ensureCapacityInternal(size + 1);  // Increments modCount!!
    elementData[size++] = e;
    return true;
}
```

## 1.2.解决ConcurentModificationException

当然要解决`ConcurentModificationException`这个问题，这个问题产生的原因就是在多线程下会出现争抢修改导致，即一个线程正在写入，另外一个线程抢去读，导致数据不一致异常。可以使用`Vector`或者使用`List<String> list = Collections.synchronizedList(new ArrayList<>());`来解决，但是，这两个都不是最好的解决办法，`Vector`是使用了`synchronized`关键字，使并发性下降，`java.util.concurrent`包下，有一个`CopyOnWriteArrayList`写时复制类可以解决这个问题，使用`CopyOnWriteArrayList`如下：

```java
List<String> list = new CopyOnWriteArrayList<>();
```

```java
public boolean add(E e) {
    final ReentrantLock lock = this.lock;
    lock.lock(); // 加锁
    try {
        Object[] elements = getArray();
        int len = elements.length;
        Object[] newElements = Arrays.copyOf(elements, len + 1); // 拷贝一份扩容1
        newElements[len] = e; // 写入
        setArray(newElements); // 复制回去
        return true;
    } finally {
        lock.unlock(); // 释放锁
    }
}
```

## 1.3.写时复制的思想

上面我们使用到了`CopyOnWriteArrayList`，也简单的看了一下源码，其中`CopyOnWrite`就是一个容器，写时复制的容器，往一个容器中添加一个新的元素的时候，不直接就添加进去当前容器，而是先将当前容器进行一个复制，往新的容器中添加一个元素，添加完元素后再将原容器的引用指向新的容器，当然，仅仅对写入操作进行加锁操作，同时可以对容器进行并发行的读，这就是读写分离的思想。

## 1.4.HashSet

`HashSet`也是线程不安全的，底层是`HashMap`，虽然底层是`HashMap`的`k-v`键值对结构，但是`HashSet`的`add`方法却是可以只添加一个元素的，不是和`HashMap`的`k-v`键值对结构冲突，而是源码中的`v`都是一样的，如下：

```java
public HashSet() {
    map = new HashMap<>();
}
```

```java
public boolean add(E e) {
    return map.put(e, PRESENT)==null; // PRESENT是一个object类型的常量
}
```

当然，既然`HashSet`线程不安全，那么`java.util.concurrent`包，肯定也会有对应的类`CopyOnWriteArraySet`。

## 1.5.HashMap

`HashMap`线程不安全是最常见的，至于为什么线程不安全，可以参看[HashMap原码解读](<http://luokangyuan.com/hashmapxue-xi-bi-ji/>)，当然`java.util.concurrent`包也少不了替代的类了，那就是` ConcurrentHashMap`。