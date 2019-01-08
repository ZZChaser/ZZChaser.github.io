---
layout: post
title: 'angular中使用oidc-client-js的问题'
date: 2019-1-7
author: ZZChaser
# cover: '/assets/img/myBg.jpg'
tags: problems
---

> angular中使用oidc-client-js

### 问题

&emsp;&emsp;当在angular中使用oidc-client-js来做JS验证，angular的变更检测一直在执行：

### 原因

&emsp;&emsp;oidc-client-js中在tokens过期前会请求新的tokens信息，angular的变更检测又是监听的异步事件，所以这就引起了angular的变更检测。

### 解决

&emsp;&emsp;在创建UserManager对象的时候，放到runOutsideAngular中执行，里面的操作就会跳过angular的变更检测。

```javascript

    this._ngZone.runOutsideAngular(() => {
      this.manager = new UserManager(settings);
    })

```