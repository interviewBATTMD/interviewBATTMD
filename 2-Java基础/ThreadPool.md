### 线程池

在面向对象编程中，创建和销毁对象是很费时间的，因为创建一个对象要获取内存资源或者其它更多资源。在Java中更是如此，虚拟机将试图跟踪每一个对象，以便能够在对象销毁后进行垃圾回收。所以提高服务程序效率的一个手段就是尽可能减少创建和销毁对象的次数，特别是一些很耗资源的对象创建和销毁，这就是”池化资源”技术产生的原因。线程池顾名思义就是事先创建若干个可执行的线程放入一个池（容器）中，需要的时候从池中获取线程不用自行创建，使用完毕不需要销毁线程而是放回池中，从而减少创建和销毁线程对象的开销。
Java 5+中的Executor接口定义一个执行线程的工具。它的子类型即线程池接口是ExecutorService。要配置一个线程池是比较复杂的，尤其是对于线程池的原理不是很清楚的情况下，因此在工具类Executors面提供了一些静态工厂方法，生成一些常用的线程池，如下所示：

- newCachedThreadPool（）：创建一个可缓存的线程池。如果线程池的大小超过了处理任务所需要的线程，那么就会回收部分空闲（60秒不执行任务）的线程，当任务数增加时，此线程池又可以智能的添加新线程来处理任务。此线程池不会对线程池大小做限制，线程池大小完全依赖于操作系统（或者说JVM）能够创建的最大线程大小。
- newFixedThreadPool（int nThreads）：创建固定大小的线程池。每次提交一个任务就创建一个线程，直到线程达到线程池的最大大小。线程池的大小一旦达到最大值就会保持不变，如果某个线程因为执行异常而结束，那么线程池会补充一个新线程。
- newSingleThreadExecutor（）：创建一个单线程的线程池。此线程池支持定时以及周期性执行任务的需求。
- newScheduledThreadPool（int corePoolSize）：创建指定大小的线程池。此线程池支持定时以及周期性执行任务的需求。
- newSingleThreadExecutor（）：创建一个单线程的线程池。这个线程池只有一个线程在工作，也就是相当于单线程串行执行所有任务。如果这个唯一的线程因为异常结束，那么会有一个新的线程来替代它。此线程池保证所有任务的执行顺序按照任务的提交顺序执行。

前三个返回ExecutorService对象，该对象代表一个线程池，可以执行Runnable对象或者Callable对象所代表的线程，后两个返回ScheduleExecutorService对象，是ExecutorService对象的子类，在指定延迟后执行线程任务。Java8 还提供了两个新方法，充分利用CPU并行能力，提供两个 work stealing 后台线程池

线程池通过shutdown（）和shutdownNow（）方法关闭，二者都不接受新的线程，前者可以等待当前线程池所有线程完成后关闭，后者立即关闭

流程：

1、调用Executor类的静态工厂方法创建一个ExecutorService对象

2、创建Runnable或Callable实现类的实例，作为线程执行任务

3、调用ExecutorService对象的submit（）方法提交Runnable或Callable实现类的实例

4、不想提交任何任务时，通过shutdown（）方法关闭线程池

### ThreadPoolExecutor的重要参数

ThreadPoolExecutor的构造函数

```java
public ThreadPoolExecutor(int corePoolSize,
                          int maximumPoolSize,
                          long keepAliveTime,
                          TimeUnit unit,
                          BlockingQueue<Runnable> workQueue,
                          ThreadFactory threadFactory,
                          RejectedExecutionHandler handler) {
        if (corePoolSize < 0 ||
            maximumPoolSize <= 0 ||
            maximumPoolSize < corePoolSize ||
            keepAliveTime < 0)
            throw new IllegalArgumentException();
        if (workQueue == null || threadFactory == null || handler == null)
            throw new NullPointerException();
        this.corePoolSize = corePoolSize;
        this.maximumPoolSize = maximumPoolSize;
        this.workQueue = workQueue;
        this.keepAliveTime = unit.toNanos(keepAliveTime);
        this.threadFactory = threadFactory;
        this.handler = handler;
    }
```

- corePoolSize：核心线程数
- - 核心线程会一直存活，及时没有任务需要执行
  - 当线程数小于核心线程数时，即使有线程空闲，线程池也会优先创建新线程处理
  - 设置allowCoreThreadTimeout=true（默认false）时，核心线程会超时关闭
- queueCapacity：任务队列容量（阻塞队列）
- - 当核心线程数达到最大时，新任务会放在队列中排队等待执行

- maxPoolSize：最大线程数
- - 当线程数>=corePoolSize，且任务队列已满时。线程池会创建新线程来处理任务
  - 当线程数=maxPoolSize，且任务队列已满时，线程池会拒绝处理任务而抛出异常
- keepAliveTime：线程空闲时间
- - 当线程空闲时间达到keepAliveTime时，线程会退出，直到线程数量=corePoolSize
  - 如果allowCoreThreadTimeout=true，则会直到线程数量=0
- allowCoreThreadTimeout：允许核心线程超时
- rejectedExecutionHandler：任务拒绝处理器
- - 两种情况会拒绝处理任务：
  - - 当线程数已经达到maxPoolSize，切队列已满，会拒绝新任务
    - 当线程池被调用shutdown()后，会等待线程池里的任务执行完毕，再shutdown。如果在调用shutdown()和线程池真正shutdown之间提交任务，会拒绝新任务
  - 线程池会调用rejectedExecutionHandler来处理这个任务。如果没有设置默认是AbortPolicy，会抛出异常
  - ThreadPoolExecutor类有几个内部实现类来处理这类情况：
  - - AbortPolicy 丢弃任务，抛运行时异常
    - CallerRunsPolicy 执行任务
    - DiscardPolicy 忽视，什么都不会发生
    - DiscardOldestPolicy 从队列中踢出最先进入队列（最后一个执行）的任务
  - 实现RejectedExecutionHandler接口，可自定义处理器

#### 队列选项

主要有3种类型的BlockingQueue：

2.1 无界队列

队列大小无限制，常用的为无界的LinkedBlockingQueue，使用该队列做为阻塞队列时要尤其当心，当任务耗时较长时可能会导致大量新任务在队列中堆积最终导致OOM。最近工作中就遇到因为采用LinkedBlockingQueue作为阻塞队列，部分任务耗时80s＋且不停有新任务进来，导致cpu和内存飙升服务器挂掉。

2.2 有界队列

常用的有两类，一类是遵循FIFO原则的队列如ArrayBlockingQueue与有界的LinkedBlockingQueue，另一类是优先级队列如PriorityBlockingQueue。PriorityBlockingQueue中的优先级由任务的Comparator决定。
使用有界队列时队列大小需和线程池大小互相配合，线程池较小有界队列较大时可减少内存消耗，降低cpu使用率和上下文切换，但是可能会限制系统吞吐量。

**注解：LinkedBlockingQueue**

原则上LinkedBlockingQueue是有界的，不过其默认的大小是Integer.MAX_VALUE

```java
	public LinkedBlockingQueue() {
        this(Integer.MAX_VALUE);
    }

    public LinkedBlockingQueue(int capacity) {
        if (capacity <= 0) throw new IllegalArgumentException();
        this.capacity = capacity;
        last = head = new Node<E>(null);
    }
```

2.3 同步移交

如果不希望任务在队列中等待而是希望将任务直接移交给工作线程，可使用SynchronousQueue作为等待队列。SynchronousQueue不是一个真正的队列，而是一种线程之间移交的机制。要将一个元素放入SynchronousQueue中，必须有另一个线程正在等待接收这个元素。只有在使用无界线程池或者有饱和策略时才建议使用该队列。

#### 可选择的饱和策略

JDK主要提供了4种饱和策略供选择。4种策略都做为静态内部类在ThreadPoolExcutor中进行实现。

3.1 AbortPolicy中止策略

该策略是默认饱和策略。

```
  public void rejectedExecution(Runnable r, ThreadPoolExecutor e) {
            throw new RejectedExecutionException("Task " + r.toString() +
                                                 " rejected from " +
                                                 e.toString());
        }12345
```

使用该策略时在饱和时会抛出RejectedExecutionException（继承自RuntimeException），调用者可捕获该异常自行处理。

3.2 DiscardPolicy抛弃策略

```
  public void rejectedExecution(Runnable r, ThreadPoolExecutor e) {
        }12
```

如代码所示，不做任何处理直接抛弃任务

3.3 DiscardOldestPolicy抛弃旧任务策略

```
    public void rejectedExecution(Runnable r, ThreadPoolExecutor e) {
            if (!e.isShutdown()) {
                e.getQueue().poll();
                e.execute(r);
            }
        }123456
```

如代码，先将阻塞队列中的头元素出队抛弃，再尝试提交任务。如果此时阻塞队列使用PriorityBlockingQueue优先级队列，将会导致优先级最高的任务被抛弃，因此不建议将该种策略配合优先级队列使用。

3.4 CallerRunsPolicy调用者运行

```
   public void rejectedExecution(Runnable r, ThreadPoolExecutor e) {
            if (!e.isShutdown()) {
                r.run();
            }
        }12345
```

既不抛弃任务也不抛出异常，直接运行任务的run方法，换言之将任务回退给调用者来直接运行。使用该策略时线程池饱和后将由调用线程池的主线程自己来执行任务，因此在执行任务的这段时间里主线程无法再提交新任务，从而使线程池中工作线程有时间将正在处理的任务处理完成。