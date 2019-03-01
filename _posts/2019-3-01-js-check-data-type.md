---
layout: post
title: '数据类型的判断'
date: 2019-03-01
author: ZZChaser
# cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: js
---

> typeof, instanceof, constructor, Object.prototype.toString.call()

### js中的数据类型
#### 简单数据类型
* Null
* Undefined
* Boolean
* Number
* String
* Symbol (es6)
  
#### 复杂数据类型
* Array 
* Object

### typeof(主要用来判断简单数据类型)

```javascript
    typeof null; //"object"
    typeof undefined;  //"undefined"
    typeof false;   //"boolean"
    typeof 8;  //"number"
    typeof "str"  //"string"
    typeof Symbol("des"); //"symbol"
    typeof [];    //"object"
    typeof {};    //"object"
    typeof function a () {};   //"function"
```
### instanceof(用来判断是否是某个类的实例，原型链向上查找)

```javascript
    [] instanceof Object;  //true
    [] instanceof Array;   //true
    {} instanceof Object;   //true

    function TestClass () {}
    const test = new TestClass();
    test instanceof TestClass;   //true
```
### constructor(取得对象的构造函数)
```javascript
    [].constructor === Array;  //true
    [].constructor ===  Object;   //false
    {}.constructor === Object;   //true

    function TestClass () {}
    const test = new TestClass();
    test.constructor === TestClass;   //true
```
### Object.prototype.toString.call()
```javascript
    Object.prototype.toString.call(5);  //"[object Number]"
    Object.prototype.toString.call(null);   //"[object Null]"
    Object.prototype.toString.call([]);   //"[object Array]"

    function TestClass () {}
    const test = new TestClass();
    Object.prototype.toString.call(test);   //"[object Object]"
```

### 参考
* <a style='color:#0A497B' href='https://webbjocke.com/javascript-check-data-types/' target='_blank'>How to better check data types in javascript</a>
* <a style='color:#0A497B' href='https://blog.csdn.net/yucihent/article/details/79652913' target='_blank'>判断一个变量是数组还是对象</a>