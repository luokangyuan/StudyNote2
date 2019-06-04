# 一、死磕Java——Lock锁

首先，知道的是`Java`中加锁的方式有两种，分别是`synchronized`和`Lock`，`synchronized`是基于JVM层面实现的，而`Lock`是基于`JDK`层面实现的。了解锁之前先简单看一些锁的名字说明。

## 1.1.公平锁和非公平锁

**公平锁：**是指多个线程按照申请锁的顺序进行排队获取锁，讲究一个先来后到。

**非公平锁：**是指在多个线程下获取锁的顺序不是按照申请锁的顺序，可能最后申请锁的线程比先申请的线程先获取锁，简单讲，就是插队，在高并发的情况下，可能会造成优先级反转或者饥饿现象，就是老实排队的线程始终获取不到锁，都被其他线程插队了。

公平锁就是很公平，在并发环境中，每个线程就会排队获取锁，非公平锁就比较野蛮，上来就直接要锁，如果要不到才会老实的排队像公平锁一样。貌似公平锁要符合我们平时的生活习惯一样，排队买票，排队取钱，但是非公平锁在并发环境中也是有优点的，就是**吞吐量**比公平锁大。就是一个线程到银行排队，已经排了几十人，结果这个线程就问一下今天是周几，不需要办理业务，肯定就是非公平锁要效率高一点呀。

> 对于`synchronized`而言，也是一种非公平锁。

## 1.2.可重入锁（递归锁）

可重入锁指的是同一线程外层函数获得锁之后，函数内层的加锁的同步方法就会自动获取锁，也就是说，线程可以进入任何一个它已经拥有的锁的同步着的代码块。可重入锁的最大作用就是为了避免死锁。

## 1.3.自选锁

自选锁是指尝试获取锁的线程不会立即阻塞，而是采用循环的方式去尝试获取锁，这样的好处是减少线程上下文切换的耗时，但是缺点就是比较消耗`CPU`，因为会一直循环获取锁。

## 1.4.读写锁

**独占锁：**指该锁一次只能被一个线程所持有，`ReentrantLock`和`Sunchronized`都是独占锁。

**共享锁：**是指锁可以被多个线程所持有。`ReentrantReadWriteLock`中的读锁是共享锁，写锁是独占锁。

读锁的是共享锁可以保证的是并发性的提高。

## 1.5.ReentrantLock

`ReentrantLock`是`java.util.concurrent`包下的一个可重入的非公平锁。`java.util.concurrent.locks`包下的类实现如下：

![image-20190505222957607](http://image.luokangyuan.com/2019-05-05-143004.png)

`ReentrantLock`加锁的时候默认是非公平锁，

```java
Lock lock = new ReentrantLock(); // 非公平锁
Lock lock = new ReentrantLock(true); // 公平锁
```

`ReentrantLock`构造函数源码如下：

```java
public ReentrantLock(boolean fair) {
    sync = fair ? new FairSync() : new NonfairSync();
}
```

使用`ReentrantLock`表明什么是可重复入锁，代码如下：

```java
Lock lock = new ReentrantLock();
public void get() {
    new Thread(() -> {
        System.out.println("线程执行开始调用加锁的方法");
        doOther();
    }, "T1").start();
}

public void doOther() {
    try {
        lock.lock();
        doAny();
        System.out.println("doOther方法被调用");
    } finally {
        lock.unlock();
    }
}

public void doAny() {
    try {
        lock.lock();
        System.out.println("doAny方法被调用");
    } finally {
        lock.unlock();
    }
}
```

​	说明：当线程调用`doOther`方法的时候获取到锁的时候，执行`doOther`方法里面的同样加锁的同步方法`doAny()`时候就使用当前的锁，不需要再去获取锁，所以就可以继续执行下去。