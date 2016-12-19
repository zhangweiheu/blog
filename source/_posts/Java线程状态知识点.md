---
title: Java线程状态知识点
tags: [Java多线程]
categories: [Java]
comments: true
toc: true
date: 2016-04-18 16:34:52
description: 线程状态知识
---
##  Thread和Runnable实现线程的异同

###  相同点
- 都是多线程实现的方式

###  不同点
- Thread是类，而Runnable是接口；Thread是实现了Runnable接口的类。

- Runnable具有更好的扩展性，即多个线程都是基于某一个Runnable对象建立的，它们会共享Runnable对象的资源。


##  Thread类包含的start()和Run()方法的区别

- start()：它的作用是启动一个新的线程，新线程会执行相应的Run()方法；start()不能被重复调用。

- run():与普通的成员方法一样，可以被重复调用。单独调用会在当前线程中执行run(),而不会启动新线程。


##  线程状态sleep()、yield()、wait()区别

- sleep()会给其他线程运行的机会，而不考虑其他线程的优先级，因此会给较低优先级的线程一个运行的机会；yield()只会给不小于自己优先级的线程一个运行的机会。

- 当线程执行sleep()方法后，参数long millis指定睡眠时间，转到阻塞状态；当线程执行yield()方法后进入就绪状态。

- sleep()方法声明抛出InterruptedException异常，而yield()方法没有声明异常。

- sleep()比yield()方法具有更好的移植性

- 线程调用自身的sleep()或者其他线程的join()方法，进入阻塞状态，该状态停止当前线程但不释放资源，当sleep()或者join()的线程结束以后进入就绪状态

- Object.wait()对象锁，释放资源，notify()唤醒回到wait()前中断现场。

![线程状态转换](http://img.blog.csdn.net/20150827111349802)

---