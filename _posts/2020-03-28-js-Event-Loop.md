---
layout: post
title: 'Js Event Loop'
date: 2020-03-28
author: ZZChaser
# cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: js
---

> `js`使用`event loop`来处理异步事件

### 单线程的`js`
#### 什么是单线程
&emsp;&emsp;同一时间只能执行一件任务，比如学习的时候就不能玩手机。相对于多线程，就是能在多个线程中同时执行不同的任务，比如边吃零食边看电视
#### 为什么`js`采用单线程
&emsp;&emsp;`js`的主要作用就是用来和页面进行交互，如果是多线程，同一时间对同一dom元素进行操作，就不知道该执行哪一个（Html5提出了Web Worker来使用多核CUP的计算能力，允许`js`创建多线程，但创建的子线程受主线程控制，其中也不能执行Dom的相关操作）

### 任务队列
&emsp;&emsp;由于单线程的限制，后面的任务只能等待前面的完成才能执行，如果前面的任务执行时间很长，后面的就必须等待。但如果前面存在`I/O`操作（ajax发起网络请求），如果后面任务等待结果才执行的话线程就会在这段时间处于空置，所以设计者就把任务分成同步任务和异步任务。
* 同步任务：等待上一个任务执行完就立即执行
* 异步任务：不进入主线程，等待有结果后放入任务队列，主线程的同步任务执行完后，再到异步任务中取出任务执行
执行机制如下

```text
    1. 同步任务在主线程上执行，形成执行栈
    2. 异步任务返回结果，就在任务队列中放置回调任务
    3. 执行栈执行完毕就会在任务队列中取出任务进行执行
    4. 一直重复上诉三个步骤
```
因为主线程一直重复上面的步骤，所以叫做Event Loop。

### EventLoop图解
![EvevtLoop](/assets/img/eventLoop.png 'EvevtLoop')

### 参考
* <a style='color:#0A497B' href='http://www.ruanyifeng.com/blog/2014/10/event-loop.html' target='_blank'>再谈Event Loop</a>

