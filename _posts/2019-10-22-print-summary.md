---
layout: post
title: 'print summary'
date: 2019-10-22
author: ZZChaser
# cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: summary
---

> 开发打印功能的一些总结

### 1.使用第二路由
#### 大概代码(Angular)

```HTML
    <!-- html -->
    <router-outlet></router-outlet>
    <router-outlet name="print"></router-outlet>
```
```javascript
    // routing
    const routes: Routes = [
        { path: 'print',
            outlet: 'print',
            component: PrintComponent,
        }
    ];
    // function
    printDocument() {
        this.router.navigate(['/',
            { outlets: {
                'print': ['print']
            }}]
        );
    }
    onDataReady() {
        setTimeout(() => {
            window.print();
            this.router.navigate([{ outlets: { print: null }}]);
        });
    }
```
#### 优点与缺点
* 可以使用框架的语法
* 不能动态配置html

### 2.动态插入iframe
#### 大概代码
```javascript
    print(htmlStr: string, styleStr: string): void {
        const iframeDom = document.createElement('iframe');
        iframeDom.style.setAttribute('height', '100%');
        const iframeDoc = iframeDom.contentWindow.document;
        iframeDoc.body.innerHTML = htmlStr;
        const styleDom = document.createElement('style');
        styleDom.innerHTML = styleStr;
        iframeDoc.querySelector('head').appendChild(styleDom);
        
        window.print();
        iframeDom.remove();
    }
```
#### 有点与缺点
* 可以动态配置html和style
* 只能使用原生html和style

### 参考
* <a style='color:#0A497B' href='https://stackblitz.com/github/IdanCo/angular-print-service' target='_blank'>Angular第二路由实现打印</a>
