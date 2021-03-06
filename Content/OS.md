（一）进程与线程的区别

+ 进程是具有一定功能的程序关于某个数据集合上的一次运行活动，进程是系统进行资源调度和分配的一个独立单位。
+ 线程是进程的实体，是CPU调度和分派的基本单位，它是比进程更小的能独立运行的基本单位。
+ 一个进程可以有多个线程，多个线程可以并发执行。

（二）线程同步的方式

+ 互斥量：采用互斥对象机制，只有拥有互斥对象的线程才有访问公共资源的权限。因为互斥对象只有一个，所有可以保证公共资源不会被多个线程同时访问。
+ 信号量：它允许同一时刻多个线程访问同一资源，但是需要控制同一时刻访问此资源的最大线程数量。
+ 事件：通过通知操作的方式来保持多线程同步，还可以方便的实现多线程优先级的比较操作。

（三）进程的通信方式

主要分为：管道、系统IPC（消息队列，信号量，共享内存）、SOCKET

管道主要分为：普通管道PIPE、流管道、命名管道

+ 管道是一种半双工的通信方式，数据只能单向流动，并且只能在具有亲缘关系的进程间流动，进程的亲缘关系通常是父子进程
+ 命名管道也是半双工的通信方式，它允许无亲缘关系的进程间进行通信
+ 信号量是一个计数器，用来控制多个进程对资源的访问，它通常作为一个锁机制。
+ 消息队列是消息的链表，存放在内核中并且由消息队列标识符标识。
+ 共享内存就是映射一块能被其它进程访问的内存，这段内存由一个进程创建，但是多个进程可以访问。