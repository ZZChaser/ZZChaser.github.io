---
layout: post
title: 'rx.js中的subject'
date: 2019-05-31
author: ZZChaser
# cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: rx.js
---

> rx.js中的subject简单说明

#### 总体简介
&emsp;&emsp;Subject是一种特殊的Observable，允许把值播送给多个观察者。Observable都有next, error, complete方法。next可以发送多次，error和complete只会发送一次，发送后就不能再发送通知了：
1. next(v): 将值发布给观察者
    ```javascript
    const subject = new Subject();
    subject.subscribe({
      next: v => {
        console.log('1:', v);
      }
    });
    subject.subscribe({
      next: v => {
        console.log('2:', v);
      }
    });
    subject.next('aaa');
    subject.next('bbb');

    // 打印结果
    1: aaa
    2: aaa
    1: bbb
    2: bbb

    ```
2. error(err): 发送一个错误或异常
    ```javascript
    const subject = new Subject();
    subject.subscribe({
      next: v => {
        console.log('1:', v);
      },
      error: err => {
        console.log('1err:', err);
      }
    });
    subject.subscribe({
      next: v => {
        console.log('2:', v);
      },
      error: err => {
        console.log('2err:', err);
      }
    });
    subject.next('aaa');
    subject.error('not found');
    subject.next('bbb');

    // 打印结果
    1: aaa
    2: aaa
    1err: not found
    2err: not found

    ```
3. complete(): 表示完成，不再发送任何值
    ```javascript
    const subject = new Subject();
    subject.subscribe({
      next: v => {
        console.log('1:', v);
      },
      error: err => {
        console.log('1err:', err);
      },
      complete: () => {
        console.log('1complete:');
      }
    });
    subject.subscribe({
      next: v => {
        console.log('2:', v);
      },
      error: err => {
        console.log('2err:', err);
      },
      complete: () => {
        console.log('2complete:');
      }
    });
    subject.next('aaa');
    subject.complete();
    subject.next('bbb');

    // 打印结果
    1: aaa
    2: aaa
    1complete:
    2complete:
    ```

#### BehaviorSubject
&emsp;&emsp;BehaviorSubject会保存一个当前值，每当订阅的时候就会发布这个当前值。初始化时有一个默认值，第一次订阅会发布这个默认值，后面订阅会发送当前值。

```javascript
const subject = new BehaviorSubject('a');
subject.subscribe({
    next: v => {
    console.log('1:', v);
    }
});
subject.next('b');
subject.subscribe({
    next: v => {
    console.log('2:', v);
    }
});
subject.next('c');
// 打印结果
1: a
1: b
2: b
1: c
2: c
```

#### ReplaySubject
&emsp;&emsp;ReplaySubject可以缓存已经发布的值，创建ReplaySubject时可以指定缓存几个值。

```javascript
const subject = new ReplaySubject(2);
subject.subscribe({
    next: v => {
    console.log('1:', v);
    }
});
subject.next('a');
subject.next('b');
subject.next('c');
subject.next('d');
subject.subscribe({
    next: v => {
    console.log('2:', v);
    }
});
subject.next('e');
// 打印结果
1: a
1: b
1: c
1: d
2: c
2: d
1: e
2: e

```
#### AsyncSubject
&emsp;&emsp;AsyncSubject只有在执行complete()后，才会将最后一个值发布给观察者。

```javascript
cconst subject = new AsyncSubject();
subject.subscribe({
    next: v => {
    console.log('1:', v);
    }
});
subject.next('a');
subject.next('b');
subject.subscribe({
    next: v => {
    console.log('2:', v);
    }
});
subject.complete();
subject.next('c');
// 打印结果
1: b
2: b
```

### 参考
* <a style='color:#0A497B' href='https://cn.rx.js.org/manual/overview.html#h28' target='_blank'>RXJS手册</a>

