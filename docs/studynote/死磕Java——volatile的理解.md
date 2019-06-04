# 一、死磕Java——volatile的理解

## 1.1.JMM内存模型

理解`volatile`的相关知识前，先简单的认识一下JMM（`Java Memory Model`）,**JMM**是`jdk5`引入的一种`jvm`的一种规范，本身是一种抽象的概念，并不真实存在，它屏蔽了各种硬件和操作系统的访问差异，它的目的是为了解决由于多线程通过共享数据进行通信时，存在的本地内存数据不一致、编译器会对代码进行指令重排等问题。

**JMM**有关同步的规定：

* 线程解锁前，必须把共享变量的值刷新回主内存；
* 线程加锁前，必须读取主内存的**最新值**到自己的工作内存中；
* 加锁和解锁使用的是同一把锁；

关于上述规定如下图解：

![image-20190502153724126](http://image.luokangyuan.com/2019-05-02-073731.png)

**说明：**当我们在程序中`new`一个`user`对象的时候，这个对象就存在我们的主内存中，当多个线程操作主内存的`name`变量的时候，会先将`user`对象中的`name`属性进行拷贝一份到自己线程的工作内存中，自己修改自己工作内存中的属性后，再将修改后的属性值刷新回主内存，这就会存在一些问题，例如，一个线程写完，还没有写回到主内存，另一个线程先修改后写入到主内存，就会存在数据的丢失或者脏数据。所以，**JMM**就存在如下规定：

* **可见性**
* **原子性**
* **有序性**

## 1.2.Volatile关键字

`volatile`是`java`虚拟机提供的一种轻量级的同步机制，比较与`synchronized`。我们知道的事`volatile`的三大特性：

* 可见性
* 不保证原子性
* 禁止指令重排

### 1.2.1.Volatile如何保证可见性

可见性就是当多个线程操作主内存的共享数据的时候，当其中一个线程修改了数据写回主内存的时候，回立刻通知其他线程，这就是线程的可见性。先看一个简单的例子：

```java
class MyDataDemo {
    int num = 0;

    public void updateNum() {
        this.num = 60;
    }
}

public class VolatileDemo {

    public static void main(String[] args) {

        MyDataDemo myData = new MyDataDemo();

        new Thread(() -> {
            try {
                TimeUnit.SECONDS.sleep(3);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            myData.updateNum();
            System.out.println("num的值：" + myData.num);
        }, "子线程").start();

        while (myData.num == 0) {}
        System.out.println("程序执行结束");
    }
}
```

这是一个简单的示例程序，存在一个两个线程，一个子线程修改主内存的共享数据`num`的值，`main`线程使用`while`时时检测自己是否是道主内存的`num`的值是否被改变，运行程序**程序执行结束**并不会被打印，同时，程序也不会停止。这就是线程之间的不可见问题，解决方法就是可以添加`volatile关键字`，修改如下：

```java
volatile int num = 0;
```

### 1.2.2.Volatile保证可见性的原理

将`Java `程序生成汇编代码的时候，我们可以看见，当我们对添加了`volatile`关键字修饰的变量时候，会多出一条`Lock`前缀的的指令。我们知道的是`cpu`不直接与主内存进行数据交换，中间存在一个高速缓存区域，通常是一级缓存、二级缓存和三级缓存，而添加了`volatile`关键字进行操作时候，生成的`Lock`前缀的汇编指令主要有以下两个作用：

* 将当前处理器缓存行的数据写回系统内存；
* 这个写回内存的操作会使得其他CPU里缓存了该内存地址的数据无效；

> Idea查看程序的汇编指令在VM启动参数配上-XX:+UnlockDiagnosticVMOptions -XX:+PrintAssembly即可；
>
> 参考：[https://wiki.openjdk.java.net/display/HotSpot/PrintAssembly](<https://wiki.openjdk.java.net/display/HotSpot/PrintAssembly>)

在多处理器下，为了保证各个处理器的缓存是一致的，就会实现**缓存一致性**协议，**每个处理器通过嗅探在总线上传播的数据来检查自己缓存的值是不是过期**了，当处理器发现自己缓存行对应的内存地址被修改，就会将当前处理器的缓存行设置成无效状态，当处理器对这个数据进行修改操作的时候，会重新从系统内存中把数据读到处理器缓存里。

> 总结：Volatile通过缓存一致性保证可见性。

### 1.2.3.Volatile不保证原子性

**原子性：**也可以说是保持数据的完整一致性，也就是说当某一个线程操作每一个业务的时候，不能被其他线程打断，不可以被分割操作，即整体一致性，要么同时成功，要么同时失败。

```java
class MyDataDemo {
    volatile int num = 0;

    public void addNum() {
        num++;
    }
}
public class VolatileDemo {

    public static void main(String[] args) {
        MyDataDemo data = new MyDataDemo();
        for(int i = 1; i <= 20; i++) {
            new Thread(() -> {
                for (int j = 1; j < 1000; j++) {
                    data.addNum();
                }
            }, "当前子线程为线程" + String.valueOf(i)).start();
        }
        // 等待所有线程执行结束
        while (Thread.activeCount() > 2) {
            Thread.yield();
        }
        System.out.println("最终结果：" + data.num);
    }
}
```

上述代码就是在共享数据前添加了`volatile`关键字，当时，打印的最终结果几乎很难为20000，这就很充分的说明了`volatile`并不能保证数据的原子性，这里的`num++`操作，虽然只有一行代码，但是实际是三步操作，这也是为什么`i++`在多线程下是非线程安全的。

### 1.2.4.为什么Volatile不保证原子性

可以参考JMM模型的那一张图，就是主内存中存在一个`num = 0`，当其中一个线程将其修改为1，然后将其写回主内存的时候，就被挂起了，另外一个线程也将主内存的`num = 0`修改为1，然后写入后，之前的线程被唤醒，快速的写入主内存，覆盖了已经写入的1，造成了数据丢失操作，两次操作最终结果应该为2，但是为1，这就是为什么会造成数据丢失。再来看`i++`对应的字节码

![image-20190502175617528](http://image.luokangyuan.com/2019-05-02-095808.png)

简单翻译一下字节码的操作：

* **aload_0**：从局部变量表的相应位置装载一个对象引用到操作数栈的栈顶；
* **dup**：复制栈顶元素；
* **getfield**：先获得原始值；
* **iadd**：进行+1操作；
* **putfield**：再把累加后的值写回主内存操作；

### 1.2.5.解决Volatile不保证原子性的问题

使用`AtomicInteger`来保证原子性，有关`AtomicInteger`的详细知识，后面在死磕，官方文档截图如下：

![image-20190502182016318](http://image.luokangyuan.com/2019-05-02-102021.png)

修改之前的不保证原子性的代码如下：

```java
class MyDataDemo {
    AtomicInteger atomicInteger = new AtomicInteger();

    public void addAtomicInteger() {
        atomicInteger.getAndIncrement();
    }
}
public class VolatileDemo {

    public static void main(String[] args) {
        MyDataDemo data = new MyDataDemo();
        for(int i = 1; i <= 20; i++) {
            new Thread(() -> {
                for (int j = 1; j <= 1000; j++) {
                    data.addAtomicInteger();
                }
            }, "当前子线程为线程" + String.valueOf(i)).start();
        }
        // 等待所有线程执行结束
        while (Thread.activeCount() > 2) {
            Thread.yield();
        }
        System.out.println("最终结果：" + data.atomicInteger);
    }
}
```

### 1.2.6.Volatile的禁止指令重排序

首先，假如写了如下代码

![carbon](http://image.luokangyuan.com/2019-05-02-103258.png)

在程序中，我们觉得是会依次顺序执行，但是在计算机在执行程序的时候，为了提高性能，编译器和和处理器通常会对指令进行**指令重排序**，可能执行顺序为：2—1—3—4，也可能是：1—3—2—4，一般分为下面三种：

![image-20190502184808400](http://image.luokangyuan.com/2019-05-02-104812.png)

虽然处理器会对指令进行重排，但是同时也会遵守一些规则，例如上述代码不可能重排后将第四句代码第一个执行，所以，单线程下确保程序的最终执行结果和顺序执行结一致，这就是处理器在进行指令重排序时候必须考虑的就是指令之间的**数据依赖性**。

但是，在多线程环境下，由于编译器重排的存在，两个线程使用的变量能否保证一致性无法确定，所以结果就无法一致。在看一个示例：

![http://image.luokangyuan.com/2019-05-02-113323.png](http://image.luokangyuan.com/2019-05-02-113422.png)

在多线程环境下，第一种就是顺序执行init方法，先将`num`进行赋值操作，在执行`update`方法，结果：`num`为6，但是存在编译器重排，那么可能先执行`falg = true;`再执行`num = 1;`，最终`num`为5；

### 1.2.7.Volatile禁止指令重排序的原理

前面说到了`volatile`禁止**指令重排优化**，从而避免在多线程环境下出现结果错乱的现象。这是因为在`volatile`会在指令之间插入一条**内存屏障**指令，通过内存屏障指令告诉`CPU`和编译器不管什么指令，都不进行指令重新排序。也就说说**通过插入的内存屏障禁止在内存屏障前后的指令执行指令重新排序优化**。

什么是**内存屏障**

**内存屏障**是一个`CPU`指令，他的作用有两个：

* 保证特定操作的执行顺序；
* 保证某些变量的内存可见性；

将上述代码修改为：

```java
volatile int num = 0;

volatile boolean falg = false;
```

这样就保证执行`init`方法的时候一定是先执行`num = 1;`再执行`falg = true;`，就避免的了结果出错的现象。

## 1.3.Volatile的单例模式

```java
public class SingletonDemo {

    private static volatile SingletonDemo instance = null;

    private SingletonDemo(){};

    public static SingletonDemo getInstance() {
        if (instance == null) {
            synchronized (SingletonDemo.class) {
                if (instance == null) {
                    instance = new SingletonDemo();
                }
            }
        }
        return instance;
    }
}
```
