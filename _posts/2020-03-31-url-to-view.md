---
layout: post
title: '从url到页面'
date: 2020-03-31
author: ZZChaser
# cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: js
---

> 在浏览器中输入Url到页面渲染的过程，简单整理

### 1.DNS寻址
* 浏览器缓存 > 本地缓存 > host > DNS域名服务器

### 2.tcp/ip请求
* 三次握手
* 传输数据
* 四次挥手

### 3.解析页面
* 解析html，生成dom树（触发DOMContentLoaded）
* 解析css，生成css规则树（不阻塞dom树的构建）
* 合并dom树和css规则，生成render树（render树的渲染会等到css规则树生成后开始）
* 布局render树（reflow），计算元素尺寸、位置
* 绘制render树，绘制页面
* 浏览器将各层信息发送给GPU，GPU会将各层合成（composite），显示在屏幕上（随后触发load）

### 参考
* <a style='color:#0A497B' href='https://zhuanlan.zhihu.com/p/34453198' target='_blank'>从输入URL到页面加载的过程</a>

