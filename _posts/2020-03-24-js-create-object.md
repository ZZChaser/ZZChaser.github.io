---
layout: post
title: '创建对象'
date: 2020-03-24
author: ZZChaser
# cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: js
---

> js创建对象的几种方式

### 工厂模式
&emsp;&emsp;解决多个相似对象的创建
```javascript
    function createPerson(name, age) {
        const o = new Object();
        o.name = name;
        o.age = age;
        o.sayName = () => {
            console.log(o.name);
        }
        return o;
    }
    const zhangSan = createPerson(`张三`,18);
    zhangSan.sayName();  // 张三
```
#### 不足
* 对象识别问题（如果需要判断是否为`createPerson`创建的对象）

### 构造函数模式
&emsp;&emsp;解决对象识别问题
```javascript
    function Person(name, age) {
        this.name = name;
        this.age = age;
        this.sayName = () => {
            console.log(this.name);
        }
    }
    const zhangSan = new Person(`张三`,18);
    zhangSan.sayName();  // 张三
```
#### 调用构造函数的过程
1. 创建一个新对象
2. 将构造函数的作用域赋给对象
3. 执行构造函数
4. 返回该对象

#### 不足
* 每次实例化一个对象都会创建相同的函数

### 原型模式
&emsp;&emsp;共享属性与方法
```javascript
    function Person() {
    }
    Person.prototype.name = '张三';
    Person.prototype.age = 18;
    Person.prototype.sayName = function() {
        console.log(this.name);
    }

    const zhangSan = new Person();
    zhangSan.sayName();    // 张三
    const lisi = new Person();
    lisi.sayName();   // 张三

```
#### 不足
* 省略了构造函数初始化参数
* 多个对象共享者属性

### 组合构造函数模式与原型模式
```javascript
    function Person(name, age) {
        this.name = name;
        this.age = age;
    }
    Person.prototype.sayName = function() {
        console.log(this.name);
    }

    const zhangSan = new Person(`张三`,18);
    zhangSan.sayName();  // 张三

```
#### 不足
* 赋值属性和封装方法分离

### 动态原型模式

```javascript
    function Person(name, age) {
        this.name = name;
        this.age = age;
        if(!Person.prototype.sayName) {
            Person.prototype.sayName = function() {
                console.log(this.name);
            }
        }
    }
    
    const zhangSan = new Person(`张三`,18);
    zhangSan.sayName();  // 张三

```
### 寄生构造函数模式
&emsp;&emsp;需要为对象添加特殊的操作

```javascript
    function SpecialArr(...arrItems) {
        const arr = new Array();
        arr.push.apply(arr, arrItems);
        arr.double = function() {
            return arr.map(function(item) {
                return item * 2;
            })
        };
        return arr;
    }
    
    const arr = new SpecialArr(1,2,3,6);
    arr.double();  // [2, 4, 6, 12]
```
### 稳妥构造函数模式
&emsp;&emsp;能为属性提供安全的访问
```javascript
    function Person(name, age) {
        const o = new Object();
        o.sayName = function() {
            console.log(name);
        }
        return o;
    }
    
    const zhangSan = new Person(`张三`,18);
    zhangSan.sayName();  // 张三
    console.log(zhangSan.naem)   // undefined
```
### 参考
* <a style='color:#0A497B' target='_blank'>JavaScript高级程序第三版</a>

