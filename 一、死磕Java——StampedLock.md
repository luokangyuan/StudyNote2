# 一、死磕Java——StampedLock

`StampedLock`是`JDK1.8`新增的一种基于容量的锁，可以说是对`ReentrantReadWriteLock`锁的一种补充，`StampedLock`提供了三种模式的读写控制，简单说明如下：

* 写锁`writeLock`，是个排它锁或者叫独占锁，同时只有一个线程可以获取该锁。
* 悲观读锁`readLock`，是个共享锁，在没有线程获取独占写锁的情况下，同时多个线程可以获取该锁，如果已经有线程持有写锁，其他线程请求获取该读锁会被阻塞。
* 乐观读锁`tryOptimisticRead`。

## 1.1.StampedLock基本的使用

### 1.1.1.writeLock写锁

```java
class MyNumData {

    int num, age = 0;
    StampedLock stampedLock = new StampedLock();

    public void add(int x, int y) {
        long stamp = stampedLock.writeLock();
        try {
            num += x;
            age += y;
        } finally {
            stampedLock.unlockWrite(stamp);
        }
    }
}
```



## 1.2.StampedLock源码简单分析

### 1.2.1.Static变量含义

```java
/** Number of processors, for spin control */
private static final int NCPU = Runtime.getRuntime().availableProcessors();

/** Maximum number of retries before enqueuing on acquisition */
private static final int SPINS = (NCPU > 1) ? 1 << 6 : 0;

/** Maximum number of retries before blocking at head on acquisition */
private static final int HEAD_SPINS = (NCPU > 1) ? 1 << 10 : 0;

/** Maximum number of retries before re-blocking */
private static final int MAX_HEAD_SPINS = (NCPU > 1) ? 1 << 16 : 0;

/** The period for yielding when waiting for overflow spinlock */
private static final int OVERFLOW_YIELD_RATE = 7; // must be power 2 - 1

/** The number of bits to use for reader count before overflowing */
private static final int LG_READERS = 7;

// Values for lock state and stamp operations
private static final long RUNIT = 1L;
private static final long WBIT  = 1L << LG_READERS;
private static final long RBITS = WBIT - 1L;
private static final long RFULL = RBITS - 1L;
private static final long ABITS = RBITS | WBIT;
private static final long SBITS = ~RBITS; // note overlap with ABITS

// Initial value for lock state; avoid failure value zero
private static final long ORIGIN = WBIT << 1;

// Special value from cancelled acquire methods so caller can throw IE
private static final long INTERRUPTED = 1L;

// Values for node status; order matters
private static final int WAITING   = -1;
private static final int CANCELLED =  1;

// Modes for nodes (int not boolean to allow arithmetic)
private static final int RMODE = 0;
private static final int WMODE = 1;
```

