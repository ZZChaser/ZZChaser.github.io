---
layout: post
title: 'problems in work'
date: 2019-8-19
author: ZZChaser
# cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: summary
---

> 记录工作中遇到的一些问题，以及解决方案

1. ### new Date()在ie下的问题
```javascript
    const time = '2019-08-19 10:00:00';
    const date = new Date(time);
    console.log(date);  // chrome: Mon Aug 19 2019 10:00:00 GMT+0800 (中国标准时间)   // ie：[date] Invalid Date
```
&emsp;&emsp;ie下对Date中的参数识别只能识别'2019/08/19'格式。所以可如下处理
```javascript
    const time = '2019-08-19 10:00:00';
    const date = new Date(time.replace(/-/g,'/'));
``` 
2. ### ie样式中不支持css中的initial值，line-height改用为normal为其默认值