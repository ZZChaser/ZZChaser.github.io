---
layout: post
title: '对象继承'
date: 2020-03-25
author: ZZChaser
# cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: js
---

> js对象的继承方式

### 原型继承
&emsp;&emsp;子类的原型指向父类的实例，使子类拥有父类的属性和方法
```javascript
    function Sub() {

    }
    function Super() {
        name = '张三';
    }
    Super.prototype.name = '张三';
    Super.prototype.sayName = function() {
        console.log(this.name)
    }
    Sub.prototype = new Super();
    Sub.prototype.constructor = Sub;
    const test1 = new Sub();
    test1.sayName();  // 张三
```
#### 不足
* 共享了父类的属性

### 借用构造函数继承
&emsp;&emsp;解决共享属性的问题
```javascript
    function Sub(name) {
        Super.call(this, name);
    }
    function Super(name) {
        this.name = name;
        this.sayName = function() {
            console.log(this.name)
        }
    }
    
    Sub.prototype = new Super();
    Sub.prototype.constructor = Sub;
    const test1 = new Sub('张三');
    test1.sayName();  // 张三
```
#### 不足
* 函数在构造函数中，每次实例化都会重新创建函数

### 组合继承
&emsp;&emsp;构造函数继承属性，原型继承方法
```javascript
    function Sub(name) {
        Super.call(this, name);
    }
    function Super(name) {
        this.name = name;
    }
    Super.prototype.sayName = function() {
        console.log(this.name)
    }
    Sub.prototype = new Super();
    Sub.prototype.constructor = Sub;
    const test1 = new Sub('张三');
    test1.sayName();  // 张三

```
#### 不足
* 执行了两次父类的构造函数

### 寄生式继承
&emsp;&emsp;用来增强一个对象的功能
```javascript

    function Super(name, age) {
        this.name = name;
        this.age = age;
    }
    Super.prototype.sayName = function() {
        console.log(this.name)
    }

    function enhanceObject(obj) {
        const clone = Object.create(obj);
        clone.sayAge = function() {
            console.log(clone.age)
        }
        return clone;
    }
    
    const zhangSan = enhanceObject(new Super(`张三`,18));
    zhangSan.sayName();  // 张三
    zhangSan.sayAge();  // 18

```
#### 不足
* enhanceObject中创建的函数不能得到复用

### 寄生组合式继承
&emsp;&emsp;只用执行一次父类的构造函数
```javascript
    function Sub(name) {
        Super.call(this, name);
    }
    function Super(name) {
        this.name = name;
    }
    Super.prototype.sayName = function() {
        console.log(this.name)
    }

    function inheritProperty(supType, superType) {
        const clone = Object.create(superType.prototype);
        supType.prototype = clone;
        supType.constructor = supType;
    }
    inheritProperty(Sub, Super);

    const zhangSan = new Sub(`张三`);
    zhangSan.sayName();  // 张三

```

### 参考
* <a style='color:#0A497B' target='_blank'>JavaScript高级程序第三版</a>

