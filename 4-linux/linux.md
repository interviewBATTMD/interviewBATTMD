# Linux

## Linux用户态和内核态的理解和区别
https://blog.csdn.net/qq_39823627/article/details/78736650?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.add_param_isCf&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.add_param_isCf  
1、地址空间：用户空间+内核空间  
2、特权级不同  
3、用户态和内核态的切换：  
（1）系统调用  
（2）异常  
（3）外围设备的中断
中断和异常的异同：  
》1-最后都是由CPU发送给内核，由内核去处理  
》2-处理程序的流程设计上是相似的  
》3-产生源不同，异常是由CPU产生，中断是由硬件设备产生的  
》4-内核需要根据是异常还是中断调用不同的处理程序
》5-中断不是时钟同步的，这意味着中断可能随时到来，异常由于是CPU产生的，所以它是时钟同步的。  
》6-当处理中断时，处理中断上下文中，处理异常时，处于进程上下文中。

## 线程和进程
https://www.cnblogs.com/sky-heaven/p/8204614.html
### Linux线程实现机制
线程模型，有核心级线程和用户级线程两种线程模型，分类的标准主要是线程的调度者在核内还是在核外，前者更利于并发使用多处理器的资源，后者更多考虑的是上下文切换开销。

### 线程和进程的区别

### 多核的情况下，如何让进程运行在指定核上

### Linux进程调度器
https://blog.csdn.net/gatieme/article/details/51699889  
**2个调度器**  

### 线程同步方式有哪些？  
互斥锁，自旋锁，读写锁、条件变量  
C++ STL实现的方案：  
C++11 mutex、condition_variable
C++17 读写锁shared_mutex https://blog.csdn.net/gongjianbo1992/article/details/100061344  
C++20 信号量#include<semaphore>  

Linux系统调用pthread实现：  

### 互斥锁和自旋锁的区别，自旋锁的应用场景  
https://blog.csdn.net/qq_26093511/article/details/78634288  
从实现原理上来讲，Mutex属于sleep-waiting类型的锁。没有获得锁线程就会被阻塞。  
自旋锁属于busy-waiting类型的锁，会一直不停地去请求锁。  
自旋锁不会引起调用者阻塞，所以自旋锁的效率远高于互斥锁。  
然后自旋锁一直占用CPU，如果不能在短时间内获得锁，会使CPU效率降低。  
》》mutex导致线程阻塞  
mutex导致线程阻塞，使得线程停止执行，释放CPU，会涉及线程（上下文）切换的问题，如果频繁进行线程切换的话，对性能的消耗还是挺大的，自旋锁（这里指pthread实现，非STL11mutex模拟）则不会涉及线程切换的问题，因为一直再运行状态，但是自旋锁也会涉及内核态到用户态的转换  
》》pthread mutex实现  
https://blog.csdn.net/weixin_30348519/article/details/97269843  
从2.6.x内核之后，Linux的mutex都是futex（Fast-Usermode-muTEX）锁。  
futex（快速用户区互斥的简称）是一个在Linux上实现锁定和构建高级抽象锁如信号量和POSIX互斥的基本工具。  
Futex是由用户空间的一个对齐的整型变量和附在其上的内核空间等待队列构成，多进程或多线程绝大多数情况下对位于用户空间的futex的整型变量进行操作（汇编语言调用CPU提供的原子操作指令来增加或减少），而其他情况下，则需要通过代价较大的系统调用来对位于内核空间的等待队列进行操作（如唤醒等待的进程/线程，或将当前进程/线程放入等待队列）。除了多个线程同时竞争锁的少数情况外，基于futex的lock操作是不需要进行代价昂贵的系统调用操作的。  
这种机制的核心思想是通过将大多数情况下非同时竞争lock的操作放到在用户空间来执行，而不是代价昂贵的内核系统调用方式来执行，从而提高了效率。  

## 可重入函数和线程安全函数
可重入函数：多个执行流反复执行一个函数，其结果不会发生改变。  
线程安全函数：多个线程并发调用同一个函数时，不会出现乱序或者错误结果，我们可以说是线程安全的。  
可重入函数以一定是线程安全的，但是线程安全是可重入函数。  
例如close()是可重入函数，但是不是线程安全的，如果两个线程同时close()操作，会core dump。  
