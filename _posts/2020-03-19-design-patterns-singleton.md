---
layout: post
title: '单例模式'
date: 2020-03-19
author: ZZChaser
# cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: js
---

> 一个对象只能被实例化一次

### 对象字面量
#### 示例：
```javascript
    const testSingleton = {
        name: 'test',
        sayName: function() {
            console.log(this.name);
        }
    }
    const t1 = testSingleton;
    const t2 = testSingleton;
    console.log(t1 === t2);   // => true
```
### 闭包实现
#### 示例：

```javascript
    function TestBase() {}
    TestBase.prototype.sayHi = function () {
        console.log('hi');
    }
    const TestInstance = (function(){
        let instance;
        return function () {
            if(!instance) {
                instance = new TestBase();
            }
            return instance;
        }
    })()
    const t1 = new TestInstance();
    const t2 = new TestInstance();
    console.log(t1 === t2);    // => true
```
### ES6实现
#### 示例：

```javascript
    class TestBase {
        static instance;
        static getInstance() {
            if(!TestBase.instance) {
                TestBase.instance = new TestBase();
            }
            return TestBase.instance;
        }
        sayHi() {
            console.log('hi');
        }
    }
    const t1 = TestBase.getInstance();
    const t2 = TestBase.getInstance();
    console.log(t1 === t2); // => true
```