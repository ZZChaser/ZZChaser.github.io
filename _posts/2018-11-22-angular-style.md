---
layout: post
title: 'angular中的特殊选择器'
date: 2018-11-22
author: ZZChaser
# cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: angular
---

> <span style='margin-left:10px;color:red' >:host</span>, <span style='margin-left:10px;color:red' >:host-context</span>, <span style='margin-left:10px;color:red' >::ng-deep</span>

### :host &emsp; 选择宿主元素  
比如有如下组件:
```javascript
    @Component({
        selector: 'app-child',
        templateUrl: './child.component.html',
        style: `
            :host {
                color: blue;
            }
            :host(.second) {
                color: red;
            }
        `
    })
```
父组件中引用Child组件
``` html
    <div>
        <app-child class='first'></app-child>
        <app-child class='second'></app-child>
    </div>
```
:host选择的就是两个app-child, :host(.second)选择的是带有class为second的app-child

### :host-context &emsp; 如果祖先元素有某个class，则使用该样式。通常用于设置主题 

比如有如下子组件:
```javascript
    @Component({
        selector: 'app-child',
        template: `
            <span>子组件</span>
        `,
        style: `
            :host-context(.blue-theme) span {
                color: blue;
            }
            :host-context(.red-theme) span {
                color: red;
            }
        `
    })
```

父组件:
```javascript
    @Component({
        selector: 'app-father',
        template: `
            <div class='blue-theme'>
                蓝色主题
                <app-child></app-child>
            </div>
            <div class='red-theme'>
                红色主题
                <app-child></app-child>
            </div>
        `,
        styleUrls: ['./father.component.less']
    })
```
则蓝色主题中的span为蓝色，红色主题中的span为红色

### ::ng-deep &emsp; 用于该范围下的所有选择元素，包括子组件中的元素，一般配合:host使用。通常用于修改子组件样式

比如有如下子组件:
```javascript
    @Component({
        selector: 'app-child',
        template: `
            <span>子组件</span>
        `,
        styleUrls: ['./child.component.less']
    })
```

父组件:
```javascript
    @Component({
        selector: 'app-father',
        template: `
            <span>父组件</span>
        `,
        style: `
            :host ::ng-deep span {
                color: red;
            }
        `
    })
```
父组件和子组件的span都红色字体
