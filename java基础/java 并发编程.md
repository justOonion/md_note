# java 并发编程

[TOC]

## 线程与进程
**进程**是操作系统进行```资源```分配的最小单位，其中资源包括**CPU、内存、磁盘IO**等，同一进程中多条线程共享该进程中的全部资源，而进程和进程之间是相互独立的。
进程是程序在计算机上的一次执行活动。当你运行一个程序，你就起送一个进程。显然，程序是死的，静态的，进程是活的、动态的。进程分为系统进程和用户进程。凡是完成操作系统的各种功能的进程就是**系统进程**，**他们就是处于运行状态下的操作系统本身**，用户进程就是由你启动的进程。



###线程是CPU调度的最小单位，必须依赖于进程而存在

线程是进程的一个实体，是**CPU调度和分配**的基本单位，它是比进程更小的、能独立运行的基本单位。线程自己基本上不拥有系统资源，只拥有一点在运行中必不可少的资源（如：程序计数器、一组寄存器和栈），但是它可以和当前进程下的其他线程共享进程所有的全部资源。



###线程无处不在

任何一个程序都必须要创建线程,特别是**Java**不管任何程序都必须启动一个main函数的主线程



###CPU核心数和线程数的关系 

增加核心数目就是为了增加线程数, 一般有核心数等于并行执行的线程数！




## 线程的启动、中断

### 启动线程的方式
X extends Thread； **X.start()**;
X implements  **Runnable**;  然后**交给Thread**执行；
Thread才是java里对线程的唯一抽象，Runnable只是对任务（业务逻辑）的抽象



### 线程中止的情况

要么是**run执行结束**了，要么是抛出了一个未处理的**异常**导致线程提前结束



### suspend、resume、stop 过期方法

过期原因是这些方法在暂停、停止一个线程后，**并不会释放资源（包括锁）**



### 线程中断interrupt() 、isInterrupted()、静态方法Thread.interrupted()

安全的中止则是其他线程通过调用某个线程A的 **interrupt()** 方法对其进行中断操作，线程通过方法**isInterrupted()**来进行判断是否被中断，也可以调用静态方法**Thread.interrupted()**来进行判断当前线程是否被中断，不过**Thread.interrupted()**会同时将中断标识位改写为**false**。

**注意：处于死锁状态的线程无法被中断**



##对Java里的线程再多一点点认识
### 深入理解run()和start()
**new Thread()** 其实**只是**new出一个**Thread的实例**，还没有操作系统中真正的线程挂起钩来。只有**执行了start()**方法后，才**实现**了真正意义上的**启动线程**。
**start()**方法让一个线程**进入**就绪**队列**等待分配cpu，**分到cpu后**才调用实现的**run()**方法，**start()**方法**不能重复**调用，如果重复调用会抛出异常。
而**run**方法是**业务**逻辑实现的地方，本质上和任意一个类的任意一个成员方法并没有任何区别，可以重复执行，也可以被单独调用。

在java程序中，main线程结束 并不代表程序就运行结束了，而要看是否还有其他前台线程在运行！
新创建的线程，默认都是前台线程。只有在线程启动之前，调用setDaemon(true) 语句，这个线程才会变成后台线程。
main线程就是前台线程，main线程和所有前台线程都结束之后，程序才会退出！



### yield、 sleep
**yield** 不会释放资源，它会让出执行权；
所有执行yield()的线程有可能在进入到就绪状态后会被操作系统再次选中马上又被执行。

**sleep** 不会释放资源，会阻塞当前线程；

只有wait会释放锁，当线程从wait方法中被唤醒后，会重新去竞争锁；

### join 插队操作
**join** ：有A，B两个线程， 在B中执行A.join(), 会执行A线程，A执行完毕后，继续执行B；

###线程优先级 priority
**1～10， 默认为5** ，通过 **setPriority(int)** 方法来修改优先级
优先级高的线程分配时间片的数量要多于优先级低的线程

针对**频繁阻塞（休眠或者I/O操作）**的线程需要设置较高优先级，而**偏重计算（需要较多CPU时间或者偏运算）**的线程则设置较低的优先级，确保处理器**不被独占**。



##守护线程
**Daemon**（守护）线程是一种**支持型**线程，因为它主要被用作程序中后台调度以及支持性工作。这意味着，当一个Java虚拟机中不存在非Daemon线程的时候，Java虚拟机将会退出。可以通过调用Thread.setDaemon(true)将线程设置为Daemon线程。我们一般用不上，比如垃圾回收线程就是Daemon线程。

守护线程中的**finally**块不一定会执行





## 线程间的共享和协作
### 内置锁 synchronized
**synchronized** 可以修饰**方法**、**代码块**，它确保多个线程在同一时刻，只能有一个线程处于方法或者代码块中，它保证了线程对变量访问的可见性和排他性，又称为内置锁机制。

synchronized锁的是某个**具体的对象**  （包括类的class对象）

### volatile 最轻量的同步机制
**volatile**保证了不同线程对这个变量进行操作的可见性，即一个线程修改了变量的值，这新的值对其他线程来说是立刻可见的；
volatile 最适用的场景：一个线程写，多个线程读。

### ThreadLocal
sping事务就借助了ThreadLocal类。Spring会从数据库连接池中获得一个connection， 然后把connection放进ThreadLocal中，也就是和线程绑定了。
**Web容器中，每个完整的请求周期会由一个线程来处理**，因此，如果我们能将一些参数绑定到线程的话，就可以实现在软件架构中跨层次的参数共享（是隐式的共享）。而java中恰好提供了绑定的方法--使用ThreadLocal。
**protected**修饰的成员是让子类覆盖而设计的。

### wait、notify、notify All
[参见代码](https://github.com/justOonion/cc.multiThread)
wait 会释放锁，而且当前被唤醒后，会重新去竞争锁，锁竞争到后才会执行wait方法后面的代码
调用notify()系列方法后，对锁无影响，线程只有在syn同步代码执行完后才会自然而然的释放锁，所以notify()系列方法一般都是syn同步代码的最后一行。
**yield**、**sleep** 都不会释放锁










## 线程并发工具类
### Fork/Join
[参见代码](https://github.com/justOonion/cc.multiThread)

分而治之的思想，将大任务分为小任务，最后将小任务join起来得到结果；
###工作窃取：
每个线程会处理多个任务，当A线程处理完成后，需要等待B线程执行完成 然后合并结果；为了更高效的完成任务，A线程在处理完成之后会从B线程的任务队列的尾部获取任务执行；所以线程处理的任务队列一般是双端队列；

### ForkJoinTask
有两个类继承它 分别是**RecursiveTask**，**RecursiveAction**
RecursiveTask 用于有返回值的任务
RecursiveAction 用于没有返回值的任务
同步，异步由ForkJoinPool提交方式决定
三种提交方式 
	invoke 同步 返回join的结果
	submit 异步 返回task
	execute 异步 无返回值
	

```java
ForkJoinPool pool = new ForkJoinPool();
SumTask sumTask = new SumTask(src, 0, src.length-1);
pool.invoke(sumTask);
System.out.println(sumTask.join());
```
实现抽象方法 compute方法
拆分Task 然后invokeAll， 使用join、get获取任务结果 再合并







### CountDownLatch （await、countDown）
CountDownLatch这个类能够 **使一个线程等待其他线程完成后再执行**；

例如，主线程希望 在 **所有辅助线程** 全部执行完成之后再执行；



当一个线程结束之后，它所拥有的所有资源回被强制释放！
```java
/*模板模式*/
public AbstractCake {
	protected abstract void shape();
	protected abstract void apply();
	protected abstract void brake();
  
}
```





##线程池

1、使用线程池降低资源消耗
2、提高响应速度，节约**创建**和**销毁**的时间
3、提高线程的可管理性















