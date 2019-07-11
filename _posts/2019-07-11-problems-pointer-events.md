---
layout: post
title: 'pointer-events'
date: 2019-07-11
author: ZZChaser
# cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: problems
---

> css 属性：pointer-events

### 简述

pointer-events 属性是一个指针属性，是用于控制在什么条件下特定的图形元素可以成为指针事件的目标。有很多取值，普通元素只有 auto，none 生效，其余的是用于 svg 元素。后面只讨论普通元素

```css
pointer-events: auto | none | visiblePainted | visibleFill | visibleStroke | visible | painted | fill | stroke | all |
    inherit;
```

-   auto(默认值)：
    该元素能接受指针事件并传递事件
-   none：
    该元素以及子元素不能接受指针事件，但是能传递事件

### 例子(react)

1.

```html
<div className="grandFather" onClick={() => console.log('grandFather')}>
    <div className="father" onClick={() => console.log('father')}>
        <div className="child" onClick={() => console.log('child')}>child</div>
    </div>
</div>
```

```css
.grandFather {
    width: 90px;
    height: 90px;
    background: blue;
    .father {
        width: 70px;
        height: 70px;
        background: gray;
        .child {
            width: 50px;
            height: 50px;
            background: green;
        }
    }
}
```

点击 child 元素，三个元素是 log 都会打印。如果在 father 元素上 pointer-events: none，点击 child 只会打印 grandFather 的 log。

2.

```html
<div className="inner" onClick={() => console.log('inner')}>
    inner
</div>
<div className="outter" onClick={() => console.log('outter')}>
    outter
</div>
```

```css
.inner {
    position: absolute;
    top: 0;
    left: 0;
    width: 50px;
    height: 50px;
    background: green;
}
.outter {
    position: absolute;
    top: 0;
    left: 0;
    width: 50px;
    height: 50px;
    background: red;
}
```

当两个元素重叠在一起，给 outter 元素设置了 pointer-events: none; 触发的就是下层元素的 click 事件。
### 参考
* <a style='color:#0A497B' href='https://www.jianshu.com/p/1132f5b0e32c' target='_blank'>CSS之pointer-events属性</a>