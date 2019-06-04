<p align="center">
<img width="130" align="center" src="http://image.luokangyuan.com/Java.svg"/>
</p>
<h1 align="center">死磕Java——CAS</h1>


前面我们说到`volatile`不保证原子性，解决办法就是使用`AtomicInteger`代替`int`，但是为什么使用`AtomicInteger`就可以保证了原子性了，是因为`AtomicInteger`实现的就是`CAS思想`和`Unsafe`的支持。

## 1.1.CAS是什么

```java
AtomicInteger atomicInteger = new AtomicInteger(5);
atomicInteger.compareAndSet(5, 10);
```

**CAS**：即比较和交换（`compareAndSet`）,`CAS`的思想比较简单就是三个值：当前内存值V，旧的预期值A，和要更新的值B，当且仅当内存值V等于预期值A，才将内存值修改为B，并返回`true`，否则什么都不做，返回false。下面就以`atomicInteger.getAndIncrement();`分析一下`AtomicInteger`使用的`CAS`思想。

## 1.2.CAS的原理

**使用AtomicInteger**

```java
public static void main(String[] args) {
    AtomicInteger atomicInteger = new AtomicInteger(5);
    atomicInteger.compareAndSet(5, 10);
    atomicInteger.getAndIncrement();
}
```

**compareAndSet方法**

```java
public final int getAndIncrement() {
    return unsafe.getAndAddInt(this, valueOffset, 1);
}
```

`unsafe.getAndAddInt(this, valueOffset, 1)`说明：

* this：代表当前对象，这里就是外部`new`的`atomicInteger`对象；
* valueOffset：代表当前对象的内存地址。
* 1：就是加1。

```java
private static final Unsafe unsafe = Unsafe.getUnsafe();
private static final long valueOffset;

static {
    try {
        valueOffset = unsafe.objectFieldOffset
            (AtomicInteger.class.getDeclaredField("value"));
    } catch (Exception ex) { throw new Error(ex); }
}

private volatile int value;
```

说明：

上述代码就是根据对象的内存地址获取当前内存的值，注意的是`private volatile int value;`添加了`volatile`关键字，所以，保证了可见性。

**unsafe.getAndAddInt方法**

```java
public final int getAndAddInt(Object var1, long var2, int var4) {
    int var5;
    do {
        var5 = this.getIntVolatile(var1, var2);
    } while(!this.compareAndSwapInt(var1, var2, var5, var5 + var4));

    return var5;
}
```

说明：

* `unsafe`类是`CAS`的核心类，由于`Java`无法直接操作底层系统，只能通过`native`修饰的本地方法操作，基于`unsafe`类就可以直接操作内存中的数据。
* `getAndAddInt(Object var1, long var2, int var4)`中：`Object var1`：当前对象，`long var2`：当前对象的内存地址，`int var4`：需要更新的值，这里就是1。
* `var5 = this.getIntVolatile(var1, var2);`：就是取到当前对象的内存值；
* 假如现在存在两个线程，跑在不同的`cpu`上，内存中`atomicInteger`的原始值为5，两个线程都拷贝一份到自己的工作内存中，
* 线程A通过`getIntVolatile(var1, var2)`拿到`value`值5，这时线程A被挂起。
* 线程B也通过`getIntVolatile(var1, var2)`方法获取到`value`值5，线程B没有被挂起，并执行`compareAndSwapInt`方法比较内存值也为5，成功修改内存值为10。
* 这时线程A恢复，执行`compareAndSwapInt`方法比较，发现自己手里的值(5)和内存的值(10)不一致，说明该值已经被其它线程提前修改过了，那只能重新来一遍了。
* 重新获取`value`值，因为变量`value`被`volatile`修饰，所以其它线程对它的修改，线程A总是能够看到，线程A继续执行`compareAndSwapInt`进行比较替换，直到成功。

## 1.3.CAS的缺点

前面的代码分析中我们知道`getAndAddInt`方法有一个`do-while`循环，如果`CAS`失败，就会进行一直尝试比较，如果很长时间都不成功，就会增加`CPU`的开销。所以`CAS`的一个缺点就是**循环时间长开销大**，由于`this`表示的是当前对象，所以，存在另外一个缺点就是**只能保证一个共享变量的原子操作**。最重要的缺点就是**ABA问题**。

### 1.3.1.什么是ABA问题

前面分析`CAS`思想的时候，我们知道一个线程会先获取`Value`的值，比较和交换的时候再获取内存的值和手里的`value`进行比较，说的是如果一致就表示没有被其他线程修改过，然后就执行自己的交换操作，但是，如果，一个线程修改了，然后另外还有一个线程又修改会原来的值，这个时候一比较还是一样的，这就是**ABA**问题。简单讲就是**狸猫换太子**。如果业务中不关心中间操作，只在乎开始和结尾是否一致就可，就不必要解决ABA 问题。

### 1.3.2.使用原子引用和版本机制解决ABA问题

在`java.util.concurrent.atomic`包下存在一个`AtomicReference`类，就是原子引用，`CAS`比较的只是内存中的值，现在增加一个版本号，比较值的同时再比较版本后是否一致。使用`AtomicStampedReference`带时间戳的原子引用来解决ABA问题。

```java
 public static void main(String[] args) {
        AtomicStampedReference<Integer> reference = new AtomicStampedReference<>(10, 1);

        new Thread(() -> {
            int stamp = reference.getStamp();
            System.out.println("T1拿到的第一次的版本号：" + stamp);
            // 先暂停1秒，等T2线程拿到相同的初始版本号
            try {
                TimeUnit.SECONDS.sleep(1);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            reference.compareAndSet(10, 101, reference.getStamp(), reference.getStamp() + 1);
            System.out.println("T1线程第一次操作后的版本号为："  + reference.getStamp());
            reference.compareAndSet(101, 10, reference.getStamp(), reference.getStamp() + 1);
            System.out.println("T1线程第二次操作后的版本号为："  + reference.getStamp());
        }, "T1").start();

        new Thread(() -> {
            int stamp = reference.getStamp();
            System.out.println("T2拿到的第一次的版本号：" + stamp);
            // 先暂停3秒，等T1线程有充分的时候做一次ABA操作
            try {
                TimeUnit.SECONDS.sleep(3);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            boolean b = reference.compareAndSet(10, 2019, stamp, stamp + 1);
            System.out.println("当前内存中的最新值为：" + reference.getReference());
            System.out.println("T2线程在T1线程执行完ABA问题后在执行的结果为：" + b);
        }, "T2").start();

    }
```

 