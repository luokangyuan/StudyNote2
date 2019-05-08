# 一、死磕Java——ReentrantLock

`ReentrantLock`是`java.util.concurrent.locks`包下一个可重入的默认是非公平的锁，`ReentrantLock`类是`Lock`接口的一个使用很频繁的实现类，类结构如下图：

![image-20190508213733422](http://image.luokangyuan.com/2019-05-08-133837.png)

前面说过`JMM`模型要求的是**可见性**，**原子性**和**有序性**。解决原子性的方法也有多种，例如`synchronized`同步方法或者同步代码块，也可以使用`AtomicInteger`原子包装类解决，都知道`synchronized`加锁是最笨重的解决方法，所里，这里使用的是`ReentrantLock`加锁来实现原子性，代码如下：

```java
class MyData {
    int num = 0;
    Lock lock = new ReentrantLock();
    public void add() {
        try {
            lock.lock();
            num++;
        } finally {
            lock.unlock();
        }
    }
}
public class ReentrantLockDemo {

    private static Logger log = LoggerFactory.getLogger(ReentrantLockDemo.class);

    public static void main(String[] args) {
        MyData myData = new MyData();
        for (int i = 1; i <= 20; i++) {
            new Thread(() -> {
                for (int j = 1; j <= 1000; j++) {
                    myData.add();
                }
            }, String.valueOf(i)).start();
        }
        // 知道所有的线程执行结束
        while (Thread.activeCount() > 1) {
            Thread.yield();
        }
        log.info("结果：{}", myData.num);
    }
}
```

## 1.1.ReentrantLock的公平性

`ReentrantLock`提供了一个带参数的构造函数，来让使用者决定使用是否是公平锁。

![image-20190508215454860](http://image.luokangyuan.com/2019-05-08-135459.png)

通过源码我们可以知道，无参数就是默认为非公平锁，传入`true`表示公平锁，传入`false`表示非公平锁，源码如下

```java
// 空参默认为非公平锁 
public ReentrantLock() {
     sync = new NonfairSync();
 }
```

```java
// 根绝参数决定锁的公平性
public ReentrantLock(boolean fair) {
    sync = fair ? new FairSync() : new NonfairSync();
}
```

## 1.2.ReentrantLock的非公平锁

### 1.2.1使用ReentrantLock加锁

```java
// 创建一个非公平锁
Lock lock = new ReentrantLock();
try {
    // 加锁
    lock.lock();
} finally {
    // 释放锁
    lock.unlock();
}
```

### 1.2.2.执行的是ReentrantLock的lock方法

```java
 public void lock() {
     sync.lock();
 }
```

### 1.2.3.调用的是NonfairSync类的lock方法

```java
static final class NonfairSync extends Sync {
    private static final long serialVersionUID = 7316153563782823691L;

    /**
         * Performs lock.  Try immediate barge, backing up to normal
         * acquire on failure.
         */
    final void lock() {
        // 通过CAS思想去AQS队列中获取将State值从0变为1，即获取到锁
        if (compareAndSetState(0, 1))
            // 获取锁成功则将当前线程标记为持有锁的线程,然后直接返回
            setExclusiveOwnerThread(Thread.currentThread());
        else
            // 获取锁失败则执行该方法
            acquire(1);
    }
}
```

### 1.2.4.调用AQS的cAS方法获取锁

```java
protected final boolean compareAndSetState(int expect, int update) {
    // See below for intrinsics setup to support this
    // unsafe类是通过native方法直接操作内存数据
    return unsafe.compareAndSwapInt(this, stateOffset, expect, update);
}
```

### 1.2.5.获取锁失败执行acquire方法

```java
public final void acquire(int arg) {
    if (!tryAcquire(arg) &&
        acquireQueued(addWaiter(Node.EXCLUSIVE), arg))
        selfInterrupt();
}
```

### 1.2.6.tryAcquire尝试获取锁

```java
protected boolean tryAcquire(int arg) {
    throw new UnsupportedOperationException();
}
```

`tryAcquire`这个方法是`AQS`默认的钩子方法，不同类的有不同的实现，其中`NonfairSync`实现如下：

```java
protected final boolean tryAcquire(int acquires) {
    return nonfairTryAcquire(acquires);
}
```

```java
 final boolean nonfairTryAcquire(int acquires) {
     // 传入的acquires为1，获取当前线程
     final Thread current = Thread.currentThread();
     // 获取state变量的值,即当前锁被重入的次数
     int c = getState();
     // state为0,说明当前锁未被任何线程持有
     if (c == 0) {
         // 通过CAS思想去AQS队列中获取将State值从0变为1，即获取到锁
         if (compareAndSetState(0, acquires)) {
             // 获取锁成功则将当前线程标记为持有锁的线程,然后直接返回
             setExclusiveOwnerThread(current);
             // 返回尝试获取锁成功
             return true;
         }
     }
     // //当前线程就是持有锁的线程,说明该锁被重入了
     else if (current == getExclusiveOwnerThread()) {
         // //计算state变量要更新的值
         int nextc = c + acquires;
         if (nextc < 0) // overflow
             throw new Error("Maximum lock count exceeded");
         // //非同步方式更新state值
         setState(nextc);
         // 获取锁成功，返回结果
         return true;
     }
     // 尝试获取锁失败
     return false;
 }
```



先看一下NonfairSync类的继承图，我们可以看见一个很重要的抽象类`AbstractQueuedSynchronizer`，有关`AQS`稍后在死磕，可以说`AQS`是同步组件的基础。

![image-20190508221310426](http://image.luokangyuan.com/2019-05-08-141315.png)

