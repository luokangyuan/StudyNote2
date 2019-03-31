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





## 1.2.线程的生命周期





## 1.3.线程的同步





## 1.4.线程的通信





