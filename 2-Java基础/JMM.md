# 理解Java内存区域与Java内存模型
![image1](https://github.com/interviewBATTMD/interviewBATTMD/blob/master/2-Java%E5%9F%BA%E7%A1%80/jvm%20memory.png)

## Java内存区域
Java内存模型(即Java Memory Model，简称JMM)本身是一种抽象的概念，并不真实存在，它描述的是一组规则或规范，通过这组规范定义了程序中各个变量（包括实例字段，静态字段和构成数组对象的元素）的访问方式。

## Java内存模型概述
![image2](https://github.com/interviewBATTMD/interviewBATTMD/blob/master/2-Java%E5%9F%BA%E7%A1%80/jmm.png)

JMM与Java内存区域的划分是不同的概念层次

JMM与Java内存区域相似点:都存在共享数据区域和私有数据区域

* 在JMM中主内存属于共享数据区域，从某个程度上讲应该包括了堆和方法区

* 工作内存数据线程私有数据区域，从某个程度上讲则应该包括程序计数器、虚拟机栈以及本地方法栈。

# 硬件内存架构与Java内存模型
## 硬件内存架构
## Java线程与硬件处理器
多线程程序属于语言层面的，程序一般不会直接去调用内核线程，

取而代之的是一种轻量级的进程(Light Weight Process)，也是通常意义上的线程，

由于每个轻量级进程都会映射到一个内核线程，因此我们可以通过轻量级进程调用内核线程，进而由操作系统内核将任务映射到各个处理器，

这种轻量级进程与内核线程间1对1的关系就称为一对一的线程模型

![image3](https://github.com/interviewBATTMD/interviewBATTMD/blob/master/2-Java%E5%9F%BA%E7%A1%80/thread.png)

## Java内存模型与硬件内存架构的关系
# JMM存在的必要性
# Java内存模型的承诺
## 原子性
## 理解指令重排
### 编译器重排
### 处理器指令重排
## 可见性
## 有序性
## JMM提供的解决方案
## 理解JMM中的happens-before 原则
# volatile内存语义
## volatile的可见性
## volatile禁止重排优化
