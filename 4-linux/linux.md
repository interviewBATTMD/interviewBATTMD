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
