# synchronized的三种应用方式

* 修饰实例方法，作用于当前实例加锁，进入同步代码前要获得当前实例的锁

* 修饰静态方法，作用于当前类对象加锁，进入同步代码前要获得当前类对象的锁

* 修饰代码块，指定加锁对象，对给定对象加锁，进入同步代码库前要获得给定对象的锁。

## synchronized作用于实例方法

所谓的实例对象锁就是用synchronized修饰实例对象中的实例方法

synchronized修饰的是实例方法increase，在这样的情况下，当前线程的锁便是实例对象instance

```
public class AccountingSync implements Runnable{
    //共享资源(临界资源)
    static int i=0;

    /**
     * synchronized 修饰实例方法
     */
    public synchronized void increase(){
        i++;
    }
    @Override
    public void run() {
        for(int j=0;j<1000000;j++){
            increase();
        }
    }
    public static void main(String[] args) throws InterruptedException {
        AccountingSync instance=new AccountingSync();
        Thread t1=new Thread(instance);
        Thread t2=new Thread(instance);
        t1.start();
        t2.start();
        t1.join();
        t2.join();
        System.out.println(i);
    }
    /**
     * 输出结果:
     * 2000000
     */
}
```

一个线程 A 访问实例对象 obj1 的 synchronized 方法 f1，另一个线程 B 访问实例对象 obj2 的 synchronized 方法 f2，这样是允许的;
* 两个线程操作数据并非共享，线程安全有保障
* 两个线程操作数据共享，出现线程安全问题

```
public class AccountingSyncBad implements Runnable{
    static int i=0;
    public synchronized void increase(){
        i++;
    }
    @Override
    public void run() {
        for(int j=0;j<1000000;j++){
            increase();
        }
    }
    public static void main(String[] args) throws InterruptedException {
        //new新实例
        Thread t1=new Thread(new AccountingSyncBad());
        //new新实例
        Thread t2=new Thread(new AccountingSyncBad());
        t1.start();
        t2.start();
        //join含义:当前线程A等待thread线程终止之后才能从thread.join()返回
        t1.join();
        t2.join();
        System.out.println(i);
    }
}`
```
使用synchronized修饰了increase方法，却new了两个不同实例对象，意味着存在两个不同的实例对象锁，因此t1和t2会进入各自的对象锁，但同时又共享着共享变量i，出现线程安全问题

解决方法-->将synchronized作用于静态的increase方法，对象锁为当前类对象，无论创建多少个实例对象，但对于的类对象，有且只有一个，对象锁唯一

## synchronized作用于静态方法
1、静态成员不专属于任何一个实例对象，是类成员，因此通过class对象锁可以控制静态成员的并发操作

2、线程A调用一个实例对象obj1的非static synchronized方法，

线程B需要调用obj1所属类的静态 synchronized方法，

是允许的，不会发生互斥现象，

因为访问静态 synchronized 方法占用的锁是当前类的class对象，而访问非静态 synchronized 方法占用的锁是当前实例对象锁

## synchronized同步代码块
某些情况下，编写的方法体比较大，同时存在一些比较耗时的操作，而需要同步的代码又只有一小部分，

直接对整个方法进行同步操作，会得不偿失，

此时可以使用同步代码块的方式对需要同步的代码进行包裹，就无需对整个方法进行同步操作


# synchronized底层语义原理
## 理解Java对象头与Monitor
## synchronized代码块底层原理
## synchronized方法底层原理
## Java虚拟机对synchronized的优化
### 偏向锁
### 轻量级锁
### 自旋锁
### 锁消除

# 关于synchronized 可能需要了解的关键点
## synchronized的可重入性
## 线程中断与synchronized
### 线程中断
### 中断与synchronized
## 等待唤醒机制与synchronized
