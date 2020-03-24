---
layout: post
title: '策略模式'
date: 2020-03-23
author: ZZChaser
# cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: js
---

> 定义一系列的算法，把它们一个个封装起来，并且使它们可以相互替换

### 策略模式中如下有两部分。
* 策略类：负责具体的算法
* 环境类：接受用户的请求，把请求委托给某个策略类

### 炒菜为例：
```javascript
    // 策略类集合
    const strategyObj = {
        '番茄炒蛋': () => {
            console.log('番茄炒蛋：不用辣椒不用蒜')
        },
        '青椒土豆': () => {
            console.log('青椒土豆：少许醋')
        },
    }
    // 环境类
    const restaurant = {
        cook: (name) => {
            if (!strategyObj.hasOwnProperty(name)) {
                console.log('暂时没有该菜品');
                return;
            }
            return strategyObj[name]();
        }
    }
    restaurant.cook('番茄炒蛋');  // 番茄炒蛋：不用辣椒不用蒜

```

### 参考
* <a style='color:#0A497B' href='https://segmentfault.com/a/1190000017721211' target='_blank'>js设计模式--策略模式</a>