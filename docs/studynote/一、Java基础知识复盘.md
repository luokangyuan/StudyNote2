# 一、Java基础知识复盘-多线程

## 1.1.线程的创建和使用

### 方式一：继承Thread类

*  创建一个类继承于`Thread`类
* 重写`Thread`类的`run`方法
* 创建继承`Thread`类的子类对象
* 通过子类对象调用`start()`方法

```java
public class ThreadTest {
    public static void main(String[] args) {
        MyThread myThread = new MyThread();
        myThread.start();
        for (int i = 0; i < 1000; i++) {
            if (i % 2 == 0) {
                System.out.println("我是主线程，求偶数：" + i);
            }
        }
    }
}

class MyThread extends Thread {
    @Override
    public void run() {
        for (int i = 0; i < 1000; i++) {
            if (i % 2 != 0) {
                System.out.println("我是子线程，求奇数：" + i);
            }
        }
    }
}
```

### 方式二：实现Runnable接口

* 创建一个实现了`Runnable`接口的类。
* 实现接口中定义的`run()`抽象方法。
* 创建一个实现类的对象。
* 将实现类的对象通过参数的形式传递到`Thread`的构造方法中，以此来创建`Thread`对象。
* 通过`Thread`对象来调用`start()`方法。

```java
public class RunnableTest {
    public static void main(String[] args) {
        MyRunnableThread myRunnableThread = new MyRunnableThread();
        Thread thread = new Thread(myRunnableThread);
        thread.start();
    }
}

class MyRunnableThread implements Runnable {
    @Override
    public void run() {
        Thread.currentThread().setName("子线程");
        for (int i = 0; i < 1000; i++) {
            System.out.println(Thread.currentThread().getName() + ":" + i);
        }
    }
}
```

### 方式三：实现Callable接口

* 创建一个实现类`Callable`接口的实现类。
* 重写`call()`方法。
* 创建实现类的对象。
* 将实现`callable`接口的实现类对象作为参数传递到`FutureTask`的构造函数中。
* 创建一个`Thread`对象，并将`FutureTask`对象作为参数传递，并调用`start()`方法。
* 如果需要获取线程的返回值，就使用`get()`方法调用。

```java
public class CallableTest {
    public static void main(String[] args) {
        CallableThread callableThread = new CallableThread();
        FutureTask<Integer> futureTask = new FutureTask<Integer>(callableThread);
        new Thread(futureTask).start();
        try {
            Integer sum = futureTask.get();
            System.out.println(sum);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

class CallableThread implements Callable<Integer> {
    @Override
    public Integer call() throws Exception {
        int sum = 0;
        for (int i = 0; i < 1000; i++) {
            sum += i;
        }
        return sum;
    }
}
```

**对比方式二，有什么不同**

* 相比`run()`方法，可以有返回值。
* 这个方法可以抛出异常，实现`Runnable`接口的`run`方法只能`try-catch`。
* 支持泛型的返回值。
* 可以借助`FutureTask`类，获取返回结果等操作。

### 方式四：使用线程池

* 提供指定线程数量的线程池
* 执行指定的线程操作，需要提供实现类Runnable接口或者Callable接口的实现类对象

```java
public class ThreadPoolTest {
    public static void main(String[] args) {
        ExecutorService executorService = Executors.newFixedThreadPool(10);
        Future submit = executorService.submit(new MyThreadPool());
    }
}

class MyThreadPool implements Callable {

    @Override
    public Object call() throws Exception {
        int sum = 0;
        for (int i = 0; i < 100; i++) {
            System.out.println(Thread.currentThread().getName() + ":" + i);
            sum += i;
        }
        return sum;
    }
}
```

**说明：**

`ExecutorService`是真正的线程池接口，常见的子类有`ThreadPoolExcutor`。

`Executors`是一个工具类，用于创建不同的线程池，如下

* `Executors.newCachedThreadPool();`：创建一个可根据需要创建新线程的线程池。主要问题是线程数最大数是Integer.MAX_VALUE，可能会创建数量非常多的线程，甚至OOM。
* `Executors.newFixedThreadPool(10);`：创建一个可重用固定线程数的线程池。 主要问题是堆积的请求处理队列可能会耗费非常大的内存，甚至OOM。
* `Executors.newSingleThreadExecutor();`：创建一个只有一个线程的线程池。 主要问题是堆积的请求处理队列可能会耗费非常大的内存，甚至OOM。
* `Executors.newScheduledThreadPool(10);`：创建一个线程池，可以在定期执行。主要问题是线程数最大数是Integer.MAX_VALUE，可能会创建数量非常多的线程，甚至OOM。

尴尬的是，线程池不建议使用Executors去创建，而是通过ThreadPoolExecutor的方式。如下所示：

```java
public class ThreadPoolFactoryTest {
    private static ThreadFactory myThreadFactory = new ThreadFactoryBuilder().setNameFormat("Mythread-pool-%d").build();

    private static ThreadPoolExecutor myThreadPool = new ThreadPoolExecutor(10, 30, 10, TimeUnit.SECONDS, new LinkedBlockingDeque<Runnable>(), myThreadFactory);

    public static void main(String[] args) {
        myThreadPool.execute(new MyThread2());
    }
}

class MyThread2 implements Runnable {

    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            if (i % 2 == 0) {
                System.out.println(Thread.currentThread().getName() + ":" + i);
            }
        }
    }
}
```

**Thread类的常用方法**

* `start()`：启动当前线程，并调用`run()`方法。
* `Thread.currentThread()`：得到当前线程。
* `Thread.currentThread().getName();`：得到当前线程名称。
* `Thread.currentThread().setName("");`：给当前线程设置名称。
* `yield();`：释放当前CPU执行权，也可能释放后又被自己抢到。
* `join()`：就是在线程A中调用线程B的`join()`方法，就是线程A进入阻塞 状态，等线程B 全结束后再继续执行。
* `sleep(2000)`：当前线程睡眠时间。也就是阻塞指定时间。
* `isAlive()`：判断当前线程是否处于存活状态。
* `setPriority(Thread.MAX_PRIORITY);`：设置线程的优先级，优先级高只是说优先概念执行，并不是一定比优先级低的先执行。高优先级的线程比低优先级线程抢得CPU的概率要高一点。

## 1.2.线程的生命周期

线程的生命周期状态定义在`Thread.State`枚举类中，`NEW`、`RUNNABLE`、`BLOCKED`、`WAITING`和`TIMED_WAITING``TERMINATED`。简单的将一个线程的状态有：新建、就绪、运行、阻塞和死亡五种状态。

**生命周期图解**

![image-20190402234652085](http://image.luokangyuan.com/2019-04-02-154656.png)

## 1.3.线程的同步

线程的同步就是为类解决线程安全问题，线程安全问题就是程序在单线程和多线程分别执行后，结果不一样。这就出现了线程不安全。

### synchronized关键字

使用`synchronized`关键字，同步代码块，将操作共享数据的代码放在同步代码块中，共享数据就是指多个线程共同操作的变量。同步监视器就是锁，任何一个类的对象都可以充当锁，要求多个线程共用同一把锁。语法如下：

```javascript
synchronized (同步监视器){

}
```

```java
public class ThreadState {
    public static void main(String[] args) {
        TicketThread ticketThread1 = new TicketThread();
        Thread thread1 = new Thread(ticketThread1);
        thread1.setName("窗口一");
        thread1.start();
        Thread thread2 = new Thread(ticketThread1);
        thread2.setName("窗口二");
        thread2.start();
        Thread thread3 = new Thread(ticketThread1);
        thread3.setName("窗口三");
        thread3.start();
    }
}

class TicketThread implements Runnable {

    private int ticket = 100;

    @Override
    public void run() {
        while (true) {
            synchronized (this ) {
                if (ticket > 0) {
                    System.out.println(Thread.currentThread().getName() + "：买票，票号：" + ticket);
                    ticket--;
                } else {
                    break;
                }
            }
        }
    }
}
```





## 1.4.线程的通信


