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
2. ### ie样式中不支持css中的initial值，改用为normal为其默认值
3. ### ie中word-break: break-all无效，元素必须是block或者inline-block才会生效
4. ### ie中的sessionStorage和localStorage只在http和https网站中生效，本地文件打开为undefined
5. ### `overflow-x`和`overflow-y`在同一元素下使用问题 <a href="https://www.jb51.net/css/636587.html" target="_blank">参考</a>