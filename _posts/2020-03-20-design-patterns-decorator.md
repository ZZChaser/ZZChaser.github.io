---
layout: post
title: '装饰器模式'
date: 2020-03-20
author: ZZChaser
# cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: js
---

> 在不改变原对象的情况下，对其进行包装扩展，使其满足更复杂的功能

&emsp;&emsp;ES7语法，装饰器是一种函数，写成`@funcName`。放在类和类方法前面
#### 装饰类
```javascript
    function isDecorated(target) {
        target.isDecorated = true;
    }

    @isDecorated
    class Test {
    }
    console.log(Test.isDecorated); // => true
```
##### 参数说明
* target： 被装饰的类

#### 装饰类方法
```javascript
    function logCall( target, name, descriptor ) { /
        const oriMethord = description.value;
        description.value = function(...args) {
            console.log('call function');
            oriMethord.call(this,...args)
        }
    }
    class Test {
        @logCall
        sayName() {
            console.log('name')
        }
    }
    const t1 = new Test()
    t1.sayName();   // => call function  name

```
##### 参数说明
* target： 被装饰的函数原型
* name：要装饰的属性名
* descriptor：属性描述对象`{value: specifiedFunction,enumerable: false,configurable: true, writable: true}`

#### 添加参数
&emsp;再使用一个函数包住装饰器函数即可
```javascript
    function testDecorator(name) {
        return function(target) {
            target.name = name;
        }
    }
    @testDecorator('xiaoMing')
    class Test {
    }
    console.log(Test.name); // => xiaoMing
```
#### 多个装饰器
&emsp;先由外进入，再由内向外执行
```javascript
    function log(logData) {
        console.log(`come in ${logData}`)
        return (target, property, descriptor) => console.log('executed', logData);
    }
    class Test {
        @log(1)
        @log(2)
        sayName() {}
    }
    // => come in 1
    // => come in 2
    // => executed 2
    // => executed 1
```
#### 其它
* 装饰器不能用于函数，因为存在函数提升
* 装饰器库：<a style='color:#0A497B' href='https://github.com/jayphelps/core-decorators' target='_blank'>core-decorators.js</a>

### 参考
* <a style='color:#0A497B' href='https://es6.ruanyifeng.com/#docs/decorator' target='_blank'>阮一峰decorator</a>