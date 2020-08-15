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
Java 虚拟机中的同步(Synchronization)基于进入和退出管程(Monitor)对象实现

## 理解Java对象头与Monitor
在JVM中，对象在内存（堆内存）中的布局分为三块区域：对象头、实例数据和对齐填充

Mark Word：存储monitor引用指针

monitor对象存在于每个Java对象的对象头中(存储的指针的指向)，synchronized锁便是通过这种方式获取锁的

* 这是Java中任意对象可以作为锁的原因

* 也是notify/notifyAll/wait等方法存在于顶级对象Object中的原因

## synchronized代码块底层原理
略

## synchronized方法底层原理
略

## Java虚拟机对synchronized的优化
Java 6之后，为了减少获得锁和释放锁所带来的性能消耗，引入了轻量级锁和偏向锁

锁的状态，四种：无锁状态、偏向锁、轻量级锁、重量级锁

锁的升级是单向的，只能从低到高升级，不会出现锁的降级
### 偏向锁
* 前提：锁竞争不激烈，连续多次是同一个线程申请相同的锁

* 如果一个线程获得了锁，那么锁就进入偏向模式，此时Mark Word 的结构也变为偏向锁结构，

* 当这个线程再次请求锁时，无需再做任何同步操作，即获取锁的过程，这样就省去了大量有关锁申请的操作，从而提高程序的性能。

* 膨胀场景：锁竞争激烈，每次申请锁的线程都不相同

### 轻量级锁
* 提升程序性能的依据：对绝大部分的锁，在整个同步周期内都不存在竞争

* 所适应的场景：线程交替执行同步块的场合

* 膨胀场景：存在同一时间访问同一锁的场合

### 自旋锁
* 轻量级锁失败后使用

* 目的：避免线程真实地在操作系统层面挂起（操作系统实现线程之间的切换时需要从用户态转换到核心态）

* 过程：假设在不久将来，当前的线程可以获得锁，虚拟机让当前想要获取锁的线程做几个空循环(这也是称为自旋的原因)，可能是50个循环或100循环，在经过若干次循环后
如果得到锁，就顺利进入临界区。如果还不能获得锁，那就会将线程在操作系统层面挂起

### 锁消除
Java虚拟机在JIT编译时(可以简单理解为当某段代码即将第一次被执行时进行编译，又称即时编译)，通过对运行上下文的扫描，去除不可能存在共享资源竞争的锁

通过这种方式消除没有必要的锁，可以节省毫无意义的请求锁时间

# 关于synchronized 可能需要了解的关键点
## synchronized的可重入性
当一个线程试图操作一个由其他线程持有的对象锁的临界资源时，将会处于阻塞状态，但当一个线程再次请求自己持有对象锁的临界资源时，这种情况属于重入锁，请求将会成功

## 线程中断与synchronized
### 线程中断
3个有关线程中断的方法
```
//中断线程（实例方法）
public void Thread.interrupt();

//判断线程是否被中断（实例方法）
public boolean Thread.isInterrupted();

//判断是否被中断并清除当前中断状态（静态方法）
public static boolean Thread.interrupted();
```

中断两种情况:
* 当线程处于阻塞状态或者试图执行一个阻塞操作时，我们可以使用实例方法interrupt()进行线程中断，执行中断操作后将会抛出interruptException异常(该异常必须捕捉无法向外抛出)并将中断状态复位

* 当线程处于运行状态时，我们也可调用实例方法interrupt()进行线程中断，但同时必须手动判断中断状态，并编写中断线程的代码(其实就是结束run方法体的代码)

**1.sleep()&interrupt()**

* 线程A正在使用sleep()暂停着:Thread.sleep(100000);

* 如果要取消他的等待状态,可以在正在执行的线程里(比如这里是B)调用a.interrupt();

* 令线程A放弃睡眠操作,这里a是线程A对应到的Thread实例

* 当在sleep中时 线程被调用interrupt()时,就马上会放弃暂停的状态.并抛出InterruptedException.丢出异常的,是A线程.

**2.wait()&interrupt()**

* 线程A调用了wait()进入了等待状态,也可以用interrupt()取消.

* 不过这时候要小心锁定的问题.线程在进入等待区,会把锁定解除,当对等待中的线程调用interrupt()时,*会先重新获取锁定,再抛出异常*.在获取锁定之前,是无法抛出异常的.

**3.join()&interrupt()**

* 当线程以join()等待其他线程结束时,当它被调用interrupt()，它与sleep()时一样,会马上跳到catch块里.

* 注意，是对谁调用interrupt()方法,一定是调用被阻塞线程的interrupt方法.如在线程a中调用来线程t.join().

* 则a会等t执行完后在执行t.join后的代码,当在线程b中调用来a.interrupt()方法,则会抛出InterruptedException，当然join()也就被取消了

### 中断与synchronized
线程的中断操作对于正在等待获取的锁对象的synchronized方法或者代码块并不起作用，

也就是对于synchronized来说，如果一个线程在等待锁，那么结果只有两种:

* 获得这把锁继续执行

* 保持等待，即使调用中断线程的方法，也不会生效

## 等待唤醒机制与synchronized
```
synchronized (obj) {
       obj.wait();
       obj.notify();
       obj.notifyAll();         
 }
```
使用这3个方法时，必须处于synchronized代码块或者synchronized方法中，否则就会抛出IllegalMonitorStateException异常

原因：

* 调用notify、notifyAll、wait方法前,必须拿到当前对象的监视器monitor对象，即依赖于monitor对象

* monitor 存在于对象头的Mark Word 中

* synchronized关键字可以获取 monitor 

wait() 和sleep() 的区别：

* wait()调用，线程被暂停，释放当前持有的监视器锁(monitor)，直到有线程调用notify/notifyAll方法后方能继续执行，

* sleep()只让线程休眠并不释放监视器锁



**笔记出自https://blog.csdn.net/javazejian/article/details/72828483**
