---
layout: post
title: 'angular change detection'
date: 2018-11-30
author: ZZChaser
# cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: angular
---

> angular 中的变更检测

### 什么是变更检查
&emsp;&emsp;检查数据模型和视图之间绑定的值是否发生了改变
### 什么引起了变更检查
&emsp;&emsp;事实上就是引起了数据模型和视图间绑定的值发生变化，有如下几种情况：
* 事件：click，mouseover，keyup...
* 定时器：setTimeout，setInterval
* http请求：XmlHttpRequest

&emsp;&emsp;由上可知都是异步操作，检查值的变化就变成了检查异步操作。angular通过zone.js检查到异步事件的发生
#### zone.js
&emsp;&emsp;这里简单理解，zone.js对所有的异步操作都进行了封装，使得异步操作都能在Zone(一个全局对象，用于配置如何拦截和跟踪异步回调的规则)的执行上下文中，这样就能检查到异步操作。
#### angular中结合zone.js
&emsp;&emsp;Angular源码中，有一个class叫做ApplicationRef，它监听NgZones的onMicrotaskEmpty事件。在异步事件结束时，发布onMicrotaskEmpty，执行tick()函数，这个函数执行变更检测。
```javascript
// 真实源码的非常简化版本。
class ApplicationRef {

  changeDetectorRefs:ChangeDetectorRef[] = [];

  constructor(private zone: NgZone) {
    this.zone.onMicrotaskEmpty
      .subscribe(() => this.zone.run(() => this.tick());
  }

  tick() {
    this.changeDetectorRefs
      .forEach((ref) => ref.detectChanges());
  }
}
```
### angular检查机制（脏检查）
&emsp;&emsp;简单理解，脏检查就是存储所有变量的值，每当可能有变量改变就触发检查，将所有的值同旧值进行比较(复杂数据比较的是地址)，不相等就说明发生了改变，就需要更新相应的视图。angular首次会检查所有组件，变化检查时从根组件开始从上而下的开始检查
### 参考
* <a style='color:#0A497B' href='http://pascalprecht.github.io/slides/angular-2-change-detection-explained/#/' target='_blank'>change detection</a>
* <a style='color:#0A497B' href='https://www.jianshu.com/p/6bef681a0cae' target='_blank'>Angular中的变更检测</a>

