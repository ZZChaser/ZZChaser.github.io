---
layout: post
title: 'Event Loop'
date: 2020-03-28
author: ZZChaser
# cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: js
---

> `event loop`是处理异步事件的一种机制

### 理解浏览器的进程/线程模型
#### 浏览器
* Browser进程：浏览器主进程，负责主控、协调（一个）
* 第三方插件进程：使用第三方插件的时候就会创建一个进程
* GPU进程： 用于3D绘制（一个）
* Renderer进程（内核）：默认每个tab页一个进程（浏览器可能合并空白tab），控制页面渲染，js脚本执行，事件处理等

#### Renderer进程（内核）
* GUI线程：辅助页面渲染
* JS引擎线程：解释和执行JS（与GUI线程互斥：如果不互斥的话，当渲染和操作dom同时执行，就会造成元素不一致）
* 事件触发线程：管理任务队列(macroTask)
* 定时器触发线程：（setTimeout，setInterval）计时完毕后将回调函数插入任务队列
* 异步http请求线程：XMLHttpRequest创建成功后就会新建一个线程。检测到请求状态变更后，就会产生状态变更事件，将回调插入任务队列后

### 单线程的`js`
#### 什么是单线程
&emsp;&emsp;同一时间只能执行一件任务，比如学习的时候就不能玩手机。相对于多线程，就是能在多个线程中同时执行不同的任务，比如边吃零食边看电视
#### 为什么`js`采用单线程
&emsp;&emsp;`js`的主要作用就是用来和页面进行交互，如果是多线程，同一时间对同一dom元素进行操作，就不知道该执行哪一个（Html5提出了Web Worker来使用多核CUP的计算能力，允许`js`创建多线程，但创建的子线程受主线程控制，其中也不能执行Dom的相关操作）

### macrotask
&emsp;&emsp;又称`task`，由`事件触发线程`管理。一个Event Loop有一个或多个`Task Queue`，存放不同任务源的`task`，但对于不同的任务源的任务队列，浏览器会进行调度，允许优先执行来自特定任务源的 Task。
* setImmediate
* MessageChannel
* setTimeout
* setInterval

### microtask
&emsp;&emsp;又称`jobs`，存放在`JS引擎线程`微任务队列（Job Queues）中。一个Event Loop只有一个微任务队列（未列举Node环境情况）
* Promose
* MutationOberser

### Event Loop
1. 执行`Task`：从`Task Queue`取出第一个任务进入执行栈
2. 执行`microtask`：(如果执行栈不为空则跳过)遍历`Microtask Queue`，处理每个任务，直至队列为空
3. 进入`Update the rendering`（更新渲染）
   * 设置 Performance API 中 now() 的返回值
   * 遍历本次 Event Loop 相关的 Documents，执行更新渲染（浏览器判断是否跳过更新）
   * 确认更新后继续（触发事件 => 执行 animation frame callbacks，window.requestAnimationFrame => 更新 intersection observations，也就是 Intersection Observer API）
4. 一直重复上诉步骤
   
因为主线程一直重复上面的步骤，所以叫做Event Loop。

### EventLoop图解
![EvevtLoop](/assets/img/eventLoop.png 'EvevtLoop')

### 参考
* <a style='color:#0A497B' href='http://www.ruanyifeng.com/blog/2014/10/event-loop.html' target='_blank'>再谈Event Loop</a>
* <a style='color:#0A497B' href='https://segmentfault.com/a/1190000012925872#item-7' target='_blank'>从浏览器多进程到JS单线程</a>
* <a style='color:#0A497B' href='https://zhuanlan.zhihu.com/p/34229323' target='_blank'>深入理解 JavaScript Event Loop</a>

