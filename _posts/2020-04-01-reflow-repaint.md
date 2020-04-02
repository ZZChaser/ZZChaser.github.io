---
layout: post
title: 'reflow 和 repaint'
date: 2020-04-01
author: ZZChaser
# cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: render
---

> `reflow` 必定会引起 `repaint`, `repaint` 不一定引起 `reflow`

### reflow
&emsp;&emsp;一般是元素的内容、结构、尺寸、位置发生了变化，需要重新计算样式和render tree
#### 引起reflow的操作
* 首次渲染
* 浏览器窗口大小变化
* 元素尺寸发生变化
* 元素内容发生变化
* 元素位置发生变化
* 元素的添加或删除
* 操作某些属性
  1. offset(Top/Left/Width/Height)
  2. clent(Top/Left/Width/Height)
  3. scroll(Top/Left/Width/Height)
  4. width、height
  5. getComputedStyle()、scrollIntoView()...

### repaint
&emsp;&emsp;元素的改变只影响外观，不影响文档流中的位置时，只需应用新样式重绘改元素即可
#### 引起repaint的操作
* background改变
* color改变
* border-color改变

### 现代浏览器的优化
&emsp;&emsp;现代浏览器会有一个队列，存放引起回流和重绘的操作，到达一定数量或到达一定时间就会批量处理。但当访问`offsetTop`等属性时，会强制清空队列，以保证得到的值的最新的

### 程序中优化
* `css`中减少表达式，如`calc()`
* 减少`table`的使用，`table`中一个元素改变，会导致整个`table`回流。可以设置`table-layout`为`auto`或`fixed`优化
* 避免重复操作样式，应一次赋值所有样式，或用`class`代替
* 经常回流的元素设置`position`为`fixed`或`absolute`
* 避免重复对dom的操作，可以创建`documentFragment`，在其中操作，最后一次应用到文档中
* 将操作比较多的dom设置`display:none`，进行完所有操作后再显示。`display:none`元素上的更改不会引起`reflow`和`repaint`
* 避免频繁读取引起回流的属性，如有必要可以重放在变量中供后续使用

### 参考
* <a style='color:#0A497B' href='https://juejin.im/post/5a9923e9518825558251c96a' target='_blank'>浏览器的回流与重绘</a>

