---
layout: post
title: 'less中使用calc的问题'
date: 2018-11-21
author: ZZChaser
# cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: problems
---

> 在less中正确使用calc

### 问题

在less中使用动态计算属性calc：

``` css

    width: calc(100% - 10px);

```

结果被编译成了：

``` css

    width: 90%;

```

### 原因

less编译时就把100%-10px当成表达式计算了，所以得到了90%

### 解决

避免less的编译  
结构：~'值'

``` css

width: ~'calc(100% - 10px)';

```

或者

``` css

width: calc(~'100% - 10px');

```

有变量情况

``` css
@wid： 20px;
width: calc(~'100% - @wid');

```