---
title: Java基础知识-多线程-入门
date: 2019-02-18 13:54:19
author: fuf
notebook: blog
evernote-version: 0
source: 原创/转载
thumbnail: 
tags:
    - JAVA,多线程
blogexcerpt:
---
<!-- more -->
# 一 进程和多线程简介
## 1.1 相关概念
> 什么是进程

进程是程序的一次执行过程，是系统运行程序的基本单位，因此进程是动态的。一个进程就是一个执行中的程序。

> 什么是线程

线程是一个比线程更加小的执行单位。一个进程在执行过程中会产生多个线程。在一个进程中的同类线程共享同一块内存空间和一组系统资源，所以系统产生一个线程，或是在各个线程之间切换工作时，负担要比进程小得多，所以，线程也被称为轻量级进程。
<!-- more -->
> 进程和线程的区别

线程时进程划分成的更小的运行单位，线程和进程最大的区别时**各个进程之间时独立的，但是线程不一定**，因为同一进程中的线程极有可能会相互影响。从另一角度来说，进程属于操作系统的范畴，主要是同一段时间内，可以同时执行一个以上的程序，而线程则是在同一程序内几乎同时执行一个以上的程序段。

## 1.2 多线程
> 什么是多线程

**多线程是几乎同时执行多个线程**（一个处理器在某一个时间点上永远都只能是一个线程！即使这个处理器是多核的，除非有多个处理器才能实现多个线程同时运行。）。几乎同时是因为实际上多线程程序中的多个线程实际上是一个线程执行一会然后其他的线程再执行，并不是很多书籍所谓的同时执行。

> 为什么多线程是必要的？

1. 使用线程可以把占据长时间的程序中的任务放到后台去处理。
2. 用户界面可以更加吸引人，这样比如用户点击了一个按钮去触发某些事件的处理，可以弹出一个进度条来显示处理的进度。
3. 可能加快程序的运行速度。

# 二 使用多线程
## 2.1 继承 Thread 类
MyThread.java
```
public class MyThread extends Thread {
	@Override
	public void run() {
		super.run();
		System.out.println("MyThread");
	}
}
```
> 线程是一个子任务，CPU以不确定的方式，或者说是以随机的时间来调用线程中的run方法。

## 2.2 实现Runnable接口
> 推荐实现Runnable接口方式开发多线程，因为Java单继承但是可以实现多个接口。但是在开发实践中不应该显示的去创建线程，而是利用线程池。

MyRunnable.java
```
public class MyRunnable implements Runnable {
	@Override
	public void run() {
		System.out.println("MyRunnable");
	}
}
```

Run.java

```
public class Run {

	public static void main(String[] args) {
		Runnable runnable=new MyRunnable();
		Thread thread=new Thread(runnable);
		thread.start();
		System.out.println("运行结束！");
	}

}
```
# 三 实例变量和线程安全
> 定义线程类中的实例变量针对其他线程可以有共享和不共享之分

## 3.1 不共享数据的情况。
```
public class MyThread extends Thread {

	private int count = 5;

	public MyThread(String name) {
		super();
		this.setName(name);
	}

	@Override
	public void run() {
		super.run();
		while (count > 0) {
			count--;
			System.out.println("由 " + MyThread.currentThread().getName()
					+ " 计算，count=" + count);
		}
	}
}
```
Run.java

```
public class Run {
	public static void main(String[] args) {
		MyThread a = new MyThread("A");
		MyThread b = new MyThread("B");
		MyThread c = new MyThread("C");
		a.start();
		b.start();
		c.start();
	}
}
```
> 可以看出每个线程都有一个属于自己的实例变量count，它们之间互不影响。我们再来看看另一种情况。

## 3.2 共享数据的数据
MyThread.java
```
public class MyThread extends Thread {

	private int count = 5;

	@Override
	 public void run() {
		super.run();
		count--;
		System.out.println("由 " + MyThread.currentThread().getName() + " 计算，count=" + count);
	}
}
```
Run.java

```
public class Run {
	public static void main(String[] args) {
		
		MyThread mythread=new MyThread();
        //下列线程都是通过mythread对象创建的
		Thread a=new Thread(mythread,"A");
		Thread b=new Thread(mythread,"B");
		Thread c=new Thread(mythread,"C");
		Thread d=new Thread(mythread,"D");
		Thread e=new Thread(mythread,"E");
		a.start();
		b.start();
		c.start();
		d.start();
		e.start();
	}
}
```
可以看出这里已经出现了错误，我们想要的是依次递减的结果。为什么呢？？
因为在大多数jvm中，count--的操作分为如下下三步：
  1. 取得原有count值  
  2. 计算i -1  
  3. 对i进行赋值  
   

所以多个线程同时访问时出现问题就是难以避免的了。  

**那么有没有什么解决办法呢？**  

答案是：当然有，而且很简单。  

在run方法前加上synchronized关键字即可得到正确答案。

# 一些常用方法
1.  currentThread()
返回对当前正在执行的线程对象的引用。
2. getId()
返回此线程的标识符
3. getName()
返回此线程的名称
4. getPriority()
返回此线程的优先级
5. isAlive()
测试这个线程是否还处于活动状态。
什么是活动状态呢？
活动状态就是线程已经启动且尚未终止。线程处于正在运行或准备运行的状态。
6. sleep(long millis)
使当前正在执行的线程以指定的毫秒数“休眠”（暂时停止执行），具体取决于系统定时器和调度程序的精度和准确性。
7. interrupt()
中断这个线程。
8. interrupted() 和isInterrupted()
interrupted()：测试当前线程是否已经是中断状态，执行后具有将状态标志清除为false的功能
isInterrupted()： 测试线程Thread对相关是否已经是中断状态，但部清楚状态标志
9. setName(String name)
将此线程的名称更改为等于参数 name 。
10. isDaemon()
测试这个线程是否是守护线程。
11. setDaemon(boolean on)
将此线程标记为 daemon线程或用户线程。
12. join()
在很多情况下，主线程生成并起动了子线程，如果子线程里要进行大量的耗时的运算，主线程往往将于子线程之前结束，但是如果主线程处理完其他的事务后，需要用到子线程的处理结果，也就是主线程需要等待子线程执行完成之后再结束，这个时候就要用到join()方法了。  
    > join()的作用是：“等待该线程终止”，这里需要理解的就是该线程是指的主线程等待子线程的终止。也就是在子线程调用了join()方法后面的代码，只有等到子线程结束了才能执行
13. yield()
yield()方法的作用是放弃当前的CPU资源，将它让给其他的任务去占用CPU时间。注意：放弃的时间不确定，可能一会就会重新获得CPU时间片。
14. setPriority(int newPriority)
更改此线程的优先级

# 如何停止一个线程？
## 使用interrupt() + isInterrupted +return 停止线程

MyThread.java

```
public class MyThread extends Thread {

	@Override
	public void run() {
			while (true) {
				if (this.isInterrupted()) {
					System.out.println("ֹͣ停止了!");
					return;
				}
				System.out.println("timer=" + System.currentTimeMillis());
			}
	}

}
```

Run.java

```
public class Run {

	public static void main(String[] args) throws InterruptedException {
		MyThread t=new MyThread();
		t.start();
		Thread.sleep(2000);
		t.interrupt();
	}

}
```
# 六 线程的优先级
每个线程都具有各自的优先级，**线程的优先级可以在程序中表明该线程的重要性，如果有很多线程处于就绪状态，** 系统会根据优先级来决定首先使哪个线程进入运行状态。但这个并不意味着低  
优先级的线程得不到运行，而只是它运行的几率比较小，如垃圾回收机制线程的优先级就比较低。所以很多垃圾得不到及时的回收处理。  

**线程优先级具有继承特性**比如A线程启动B线程，则B线程的优先级和A是一样的。  
**线程优先级具有随机性**也就是说线程优先级高的不一定每一次都先执行完。  
Thread类中包含的成员变量代表了线程的某些优先级。如**Thread.MIN_PRIORITY（常数1），Thread.NORM_PRIORITY（常数5）,
Thread.MAX_PRIORITY（常数10）。其中每个线程的优先级都在Thread.MIN_PRIORITY（常数1） 到Thread.MAX_PRIORITY（常数10） 之间，在默认情况下优先级都是Thread.NORM_PRIORITY（常数5）。**

# 七 Java 多线程分类

## 多线程分类
**用户线程**：运行在前台，执行具体的任务，如程序的主线程，连接网络的子线程等都是用户线程。  
**守护线程**: 运行在后台，为其他前台线程服务，也可以说守护线程是JVM中非守护线程的“佣人”。  
  1. 特点：一旦所有的用户线程都结束运行，守护线程会随着 JVM 一起结算书工作。
  2. 应用：数据库连接池中的检测线程，JVM 虚拟机启动后的检测线程。
  3. 常见的守护线程：垃圾回收线程。

# 如何设置守护线程？

可以通过调用 **Thread 类的 setDaemon(true)** 方法设置当前的线程为守护线程。
> 注意事项

  1. setDaemon(true) 必须在 start() 方法前执行，否则会抛出IllegalThreadStateException异常。
  2. 在守护线程中产生的新线程也是守护线程。
  3. 不是所有的任务都可以分配个守护线程来执行，比如读写操作或者计算逻辑。

MyThread.java

```
public class MyThread extends Thread {
	private int i = 0;

	@Override
	public void run() {
		try {
			while (true) {
				i++;
				System.out.println("i=" + (i));
				Thread.sleep(100);
			}
		} catch (InterruptedException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}
}
```

Run.java

```
public class Run {
	public static void main(String[] args) {
		try {
			MyThread thread = new MyThread();
			thread.setDaemon(true);
			thread.start();
			Thread.sleep(5000);
			System.out.println("我离开thread对象也不再打印了，也就是停止了！");
		} catch (InterruptedException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}
}
```