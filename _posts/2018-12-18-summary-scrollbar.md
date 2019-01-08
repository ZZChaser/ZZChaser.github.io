---
layout: post
title: '遇到的滚动条'
date: 2018-12-18
author: ZZChaser
# cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: summary
---

> 项目中遇到滚动条的情况

### 滚动条需要在左边出现
&emsp;&emsp;使用direction属性改变文本的排列方向，direction: rtl
```css
  .container {
    direction: rtl;    //从右向左排列(right to left)
    //direcion: ltr;   //默认，从左到右排列(left to right)
    //direction: inherit;   //继承父元素
    //direction: initial;    //保持原有的默认值
  }
```
#### <span style='color: red'>注意：只想时滚动条出现在左边，内容正常显示，需要把container中的元素direction恢复默认</span>

### 改变滚动条样式（webkit浏览器有效）
&emsp;&emsp;webkit浏览器提供了几个关于滚动条的伪元素，可以选中调整样式
```css
  .container::-webkit-scrollbar {    //滚动条整体部分

  }
  .container::-webkit-scrollbar-button {   //滚动条两端的按钮

  }
  .container::-webkit-scrollbar-track {     //滚动条的外层轨道

  }
  .container::-webkit-scrollbar-track-piece {   //滚动条的内层轨道

  }
  .container::-webkit-scrollbar-thumb {     //滚动条中间的滑块

  }
  .container::-webkit-scrollbar-corner {    //滚动条边角

  }
  .container::-webkit-scrollbar-resizer {    //右下角拖动大小的部分

  }
```
&emsp;&emsp;简洁的一个滚动条样式
```css
  .scrollbar-y::-webkit-scrollbar{   //滚动条整体部分
      width: 5px;
      background-color: transparent;
  }

  .scrollbar-y::-webkit-scrollbar-track{   //滚动条轨道
      box-shadow: unset;
      border-radius: 10px;
      background-color:transparent;
  }
  .scrollbar-y::-webkit-scrollbar-thumb{    //滚动条里的滑块
      border-radius: 10px;
      box-shadow: inset 0 0 2px rgba(0,0,0,.3);
      background-color: rgb(162, 162, 170);
  }
```
### 参考
* <a style='color:#0A497B' href='https://blog.csdn.net/zh_rey/article/details/72473284' target='_blank'>css 设置滚动条</a>

