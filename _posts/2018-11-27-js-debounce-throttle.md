---
layout: post
title: '防抖和节流'
date: 2018-11-27
author: ZZChaser
cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: js
---

> 窗口的resize、scroll，input框的验证等。利用防抖和节流减少触发频率

### 防抖（debounce）
* 原理：在高频率触发的事件中，只在触发后的一定时间执行，如果在这个时间内还在触发事件，就重新覆盖这个定时器。
* 基本实现：
```javascript
    const debounce = (callback, wait, ...args) => {
        let timer;
        return function() {
            const context = this;
            if(timer) {
                clearTimeout(timer);
            }
            timer = setTimeout(() => {
                callback.apply(context,args);
            },wait)
        }
    }
```
* 立即执行版：
```javascript
    const debounce = (callback, wait, ...args) => {
        let timer;
        return function() {
            const context = this;
            if(timer) {
                clearTimeout(timer);
            }
            const callNow = !timer;
            timer = setTimeout(() => {
                timer = null;
            },wait);
            if(callNow) {
                callback.apply(context,args);
            }
        }
    }
```
### 节流（throttle）
* 原理：在高频率触发的事件中，只会在规定的一段时间内触发一次
* 时间戳版：
```javascript
    const throttle = (callback, wait, ...args) => {
        let pre = 0;
        return function() {
            const context = this;
            const now = Date.now();
            if(now - pre > wait){
                callback.apply(context,args);
                pre = now;
            }
        }
    }
```
* 定时器版：
```javascript
    const throttle = (callback, wait, ...args) => {
        let timer;
        return function() {
            const context = this;
            if(!timer) {
                timer = setTimeout(() => {
                    callback.apply(context,args);
                    timer = null;
                },wait);
            }
        }
    }
```
### 参考
* <a style='color:#0A497B' href='https://juejin.im/post/5b651dc15188251aa30c8669' target='_blank'>函数防抖和节流</a>

