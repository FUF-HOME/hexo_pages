---
title: Java基础知识-多线程-生产者消费者
date: 2019-03-04 00:21:02
author: fuf
notebook: blog
evernote-version: 0
source: 原创/转载
thumbnail: 
tags:
    - 默认
blogexcerpt:
---



# Java-多线程-生产者消费者
想要充分的了解一项技术，最好的方法就是动手实践，只有再实践中，才能找到这项技术的解决了什么问题，优化了那些地方。废话不多说，直接上代码。
<!-- more -->
```

public class PublicBoxQueue {

    private static final int CAPACITY = 5;

    public static void main(String[] args) {
        LinkedBlockingDeque<Integer> blockingQueue = new LinkedBlockingDeque<Integer>(CAPACITY);

        Thread producer1 = new Producer("P-1", blockingQueue, CAPACITY);
        Thread producer2 = new Producer("P-2", blockingQueue, CAPACITY);
        Thread consumer1 = new Consumer("C1", blockingQueue, CAPACITY);
        Thread consumer2 = new Consumer("C2", blockingQueue, CAPACITY);
        Thread consumer3 = new Consumer("C3", blockingQueue, CAPACITY);

        producer1.start();
        producer2.start();
        consumer1.start();
        consumer2.start();
        consumer3.start();


    }
}


class Producer extends Thread {

    private LinkedBlockingDeque<Integer> blockingDeque;
    String name;
    int maxSize;
    int i = 0;

    public Producer(String name, LinkedBlockingDeque<Integer> queue, int maxSize) {
        super(name);
        this.blockingDeque = queue;
        this.name = name;
        this.maxSize = maxSize;
    }


    @Override
    public void run() {
        while (true) {
            try {
                blockingDeque.put(i);
                System.out.println("[" + name + "] Producing value : +" + i);
                i++;

                Thread.sleep(new Random().nextInt(1000));
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

class Consumer extends Thread {

    private LinkedBlockingDeque<Integer> blockingQueue;
    String name;
    int maxSize;

    public Consumer(String name, LinkedBlockingDeque<Integer> queue, int maxSize) {
        super(name);
        this.name = name;
        this.blockingQueue = queue;
        this.maxSize = maxSize;
    }


    @Override
    public void run() {
        while (true) {
            try {
                int x = blockingQueue.take();
                System.out.println("[" + name + "] Consuming : " + x);
                Thread.sleep(new Random().nextInt(1000));

            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

```
输出日志

```
[P-1] Producing value : +0
[C2] Consuming : 0
[C1] Consuming : 0
[P-2] Producing value : +0
[P-2] Producing value : +1
[C3] Consuming : 1
[P-2] Producing value : +2
[C1] Consuming : 2
[P-2] Producing value : +3
[C3] Consuming : 3
[P-2] Producing value : +4
[C1] Consuming : 4
[C2] Consuming : 5
[P-2] Producing value : +5
[P-1] Producing value : +1
[C1] Consuming : 1
[P-1] Producing value : +2
[C3] Consuming : 2
[P-2] Producing value : +6
[C1] Consuming : 6
[P-2] Producing value : +7
[C2] Consuming : 7
[P-2] Producing value : +8
[C3] Consuming : 8
[P-1] Producing value : +3
[C2] Consuming : 3
[P-1] Producing value : +4
[C1] Consuming : 4
[P-2] Producing value : +9
[C1] Consuming : 9
[C3] Consuming : 10
[P-2] Producing value : +10
[P-1] Producing value : +5
[C1] Consuming : 5
[P-2] Producing value : +11
[C3] Consuming : 11
[P-2] Producing value : +12
[C2] Consuming : 12
[P-2] Producing value : +13
[C1] Consuming : 13
[P-2] Producing value : +14
[C2] Consuming : 14
[P-1] Producing value : +6
[C2] Consuming : 6
[P-2] Producing value : +15
[P-1] Producing value : +7
[P-2] Producing value : +16
```