---
layout: post
title: 'Web Worker的使用'
date: 2019-1-9
author: ZZChaser
# cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: summary
---

> Web Worker的使用

### Web Woker简介
&emsp;&emsp;js是单线程的，随着多核CPU的出现，单线程就没法充分利用计算机的能力。Web Worker就是在js环境中创造多线程。计算密集或者高延迟的任务就可以放在Web Worker线程中，就不会阻塞和影响主线程了。Worker线程创建成功后就会一直运行，所以在完成了任务后要及时关闭。

### 使用Web Worker的注意点
1. 同源策略：Worker的脚本文件必须与主线程的脚本文件同源。
2. DOM限制：Worker作用域中不能操作DOM对象，也无法访问document、window、parent对象。可以访问navigator和location对象。
3. 文件限制：Worker无法读取本地文件，即不能打开本机的文件系统（file://），它所加载的脚本，必须来自网络。

### 基本用法
&emsp;&emsp;主线程：
```javascript
    const worker = new Worker('work.js');  //创建web worker
    worker.postMessage({from:'main',message:'create success'});   //向worker线程发送消息
    worker.onmessage = event => {      //onmessage监听接收worker线程发回来的消息
        console.log('from worker:',event.data);
        doSomething(event.data)
    };
    function doSomething(data) {
        //接受消息后的操作
        worker.terminate();    //关闭worker线程
    }

```
&emsp;&emsp;worker线程：
```javascript
    //worker中self指向worker线程
    self.onmessage = event => {    //onmessage监听接收主线程发送的消息
        console.log('from main:',event.data);
        doSomething(event.data)
    };
    function doSomething(data) {
        //接受消息后的操作
        self.postMessage({from:'worker',message:'result done'});   //向worker线程发送消息
        //worker.close();    //worker线程中关闭自己
    }

```

### webpack打包项目中
#### 方法一：使用Blob创建worker文件
&emsp;&emsp;创建worker文件
```javascript
    const worker = () => {
        self.onmessage = (event) => {
            console.log("msg", event.data);
        };
        self.postMessage({foo: "bar"});
    };

    let code = worker.toString();
    code = code.substring(code.indexOf("{")+1, code.lastIndexOf("}"));
    const blob = new Blob([code], {type: "application/javascript"});

    export URL.createObjectURL(blob);
```
&emsp;&emsp;引用的地方
```javascript
    import Worker_script from './my.worker.js';
    const myWorker = new Worker(Worker_script);
```
#### 方法二：安装worker-loader依赖（$ npm install -D worker-loader）
&emsp;&emsp;引用的文件
```javascript
    const MyWorker = require("worker-loader!./file.js");
    const worker = new MyWorker();
```
&emsp;&emsp;或者在webpack中配置(以.worker.js结尾的文件都使用worker-loader处理)
```javascript
    {
        module: {
            rules: [
                {
                    // 匹配 *.worker.js
                    test: /\.worker\.js$/,
                    use: {
                        loader: 'worker-loader',
                    }
                }
            ]
        }
    }
```

### 参考
* <a style='color:#0A497B' href='http://www.ruanyifeng.com/blog/2018/07/web-worker.html' target='_blank'>Web Worker 使用教程</a>
* <a style='color:#0A497B' href='https://blog.csdn.net/m0_37972557/article/details/80445285' target='_blank'>如何在Webpack打包的项目中使用Web Worker</a>
* <a style='color:#0A497B' href='https://github.com/facebook/create-react-app/issues/1277' target='_blank'>Is it possible to use load webworkers?</a>
* <a style='color:#0A497B' href='https://stackoverflow.com/questions/10343913/how-to-create-a-web-worker-from-a-string' target='_blank'>How to create a Web Worker from a string</a>
