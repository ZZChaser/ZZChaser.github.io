---
layout: post
title: '观察者模式'
date: 2020-03-23
author: ZZChaser
# cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: js
---

> 定义了一种一对多的关系，让多个观察者对象同时监听某一个主题对象，这个主题对象的状态发生变化时就会通知观察者对象，使得它们能够自动更新自己

### 观察者模式中如下有两部分。
* 被观察者：分发消息
* 观察者：订阅消息，被动触发

```javascript
    // 被观察者
    class Publisher {
        private messagesMap = new Map<string, ((data?: any) => void)[]>();
        // 添加监听消息
        public AddMessageListener (type: string, listener: (data?: any) => void) {
            let listeners = this.messagesMap.get(type);
            if (listeners) {
            listeners.push(listener);
            } else {
            listeners = [listener];
            }
            this.messagesMap.set(type, listeners);
        }
        // 取消监听
        public removeMessageListener (type: string, listener: (data?: any) => void) {
            let listeners = this.messagesMap.get(type);
            if (!listeners) {
            return;
            } else {
            listeners = listeners.filter(item => item !== listener);
            }
            this.messagesMap.set(type, listeners);
        }
        // 触发消息，执行回调函数
        public emit(type: string, data?: any): void {
            const funcs = this.messagesMap.get(type);
            if (!funcs) {return}
            for(const func of funcs) {
            func(data);
            }
        }
    }
    const publisher = new Publisher();
    // 观察者
    const observer1 = {
        addLoginMessage: () => {
            publisher.AddMessageListener('login', () => {
                console.log('observer1');
            })
        }
    };
    observer1.addLoginMessage();
    // 观察者
    const observer2 = {
         addLoginMessage: () => {
            publisher.AddMessageListener('login', () => {
                console.log('observer2');
            })
        }
    };
    observer2.addLoginMessage();
    // 分发消息
    publisher.emit('login');
    // observer1
    // observer2

```

### 观察模式与订阅发布模式
* 观察者模式是有观察者与被观察者两部分，订阅发布模式会有第三个部分（消息中心）。订阅与发布消息都是通过消息中心完成
* 观察者模式只是减少了耦合，订阅发布模式则是完全解耦

### 参考
* <a style='color:#0A497B' href='https://segmentfault.com/a/1190000012547812' target='_blank'>JS 观察者模式</a>