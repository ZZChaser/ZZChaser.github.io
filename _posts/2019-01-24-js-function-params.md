---
layout: post
title: '函数传参方式'
date: 2019-01-24
author: ZZChaser
# cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: react
---

> 函数的传参方式
&emsp;&emsp;在js中函数传参有两种方式，按值传递和按共享传递

### 按值传递
&emsp;&emsp;基本数据类型按值传递
```javascript
    let name = 'zhao';
    function changeName(name) {
        name = 'zhaozhao';
    };
    changeName(name);
    console.log(name);    //zhao
```
### 按共享传递
&emsp;&emsp;复杂数据类型是按共享传递(传递的是引用的一份拷贝)
#### 测试1：
```javascript
    let obj = {
        title: 'test'
    };
    function changeTitle(obj) {
        obj.title = 'changed'
    }
    changeTitle(obj);
    console.log(obj);    //{title:'changed'}
```
#### 测试2：
```javascript
    let obj = {
        title: 'test'
    };
    function changeTitle(obj) {
        obj = {
            title: 'aaa',
            des: 'bbb'
        };
    }
    changeTitle(obj);
    console.log(obj);    //{title:'test'}
```

### 参考
* <a style='color:#0A497B' href='https://www.cnblogs.com/open-wang/p/5208606.html' target='_blank'>js函数中参数的传递</a>