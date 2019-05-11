# 一、死磕Java——CountDownLatch

`CountDownLatch`是多线程中的计数器，计数器的初始值为线程的数量,可以用于判断线程是否全部执行结束，当然前面一些文章中，也使用了其他方法来判断线程是否全部结束，下面都简单的使用一下：

## 1.1.CountDownLatch示例

```java
public static void main(String[] args) throws Exception {

    CountDownLatch countDownLatch = new CountDownLatch(6);

    for (int i = 1; i <= 6; i++) {
        new Thread(() -> {
            System.out.println(Thread.currentThread().getName());
            countDownLatch.countDown();
        }, String.valueOf(i)).start();
    }
    countDownLatch.await();
    System.out.println("所有的子线程结束完后，在继续执行main线程");
}
```

## 1.2.CountDownLatch源码分析

### 1.2.1.构造方法

```java
public CountDownLatch(int count) {
    if (count < 0) throw new IllegalArgumentException("count < 0");
    this.sync = new Sync(count);
}
```

