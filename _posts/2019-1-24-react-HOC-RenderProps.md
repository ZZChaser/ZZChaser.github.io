---
layout: post
title: 'HOC and Render Props'
date: 2019-1-24
author: ZZChaser
# cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: react
---

> HOC and Render Props
&emsp;&emsp;这两种模式都是抽出组件共同的功能进行封装

### 高阶组件 HOC(Higher Order Component)
&emsp;&emsp;传入一个组件，返回一个增强版的组件。如下简单例子，给传入组件添加name属性的一个高阶组件：
```javascript
    import React from 'react';

    function addNameProp(WrapperComponent) {
        return class extends React.Component {
            render() {
                const { name = 'defaultName', ...props } = this.props;
                return <WrapperComponent {...props, name} />
            }
        }
    }
    export default addNameProp;
```
### Render Props
&emsp;&emsp;把需要的数据传给你，显示由你自己定义。如下：
```javascript
    import React from 'react';

    class AddNameProp extends React.Component {

        render() {
            const name = 'defaultName';
            return this.props.children(name);
        }
    }

    <AddNameProp>
        {
            name => <User userName={name}><User>
        }
    </AddNameProp>

```

### HOC vs RenderProps
1. HOC传入的组件中的props可能有命名冲突
2. HOC函数可以传入更多的参数，增强实用性（RenderProps也可以通过传入props来实现）

### 参考
* <a style='color:#0A497B' href='https://www.jianshu.com/p/ff6b3008820a?utm_campaign=maleskine&utm_content=note&utm_medium=seo_notes&utm_source=recommendation' target='_blank'>React组件Render Props VS HOC 设计模式</a>
* <a style='color:#0A497B' href='https://www.cnblogs.com/cnyball/p/9313465.html' target='_blank'>可复用 React 的 HOC 以及的 Render Props</a>