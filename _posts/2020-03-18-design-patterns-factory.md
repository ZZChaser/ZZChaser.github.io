---
layout: post
title: '工厂模式'
date: 2020-03-18
author: ZZChaser
# cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: summary
---

> 工厂模式的三种实现。在不同对象中，具有相同的属性、方法，只是具体实现不一样，就应该考虑使用工厂模式

### 简单工厂模式
#### 只有一个工厂类，通过不同的参数实例化不同的对象类，这些类有共同的父类或接口
#### 示例：

```javascript
    interface IPhone {
        call(): void;
    }

    class androidPhone implements IPhone {
        call() {
            console.log('android call');
        }
    }
    class iosPhone implements IPhone {
        call() {
            console.log('ios call');
        }
    }
    class mobilePhoneFactory {
        static phoneMaps = new Map([
            ['android', androidPhone],
            ['ios', iosPhone],
        ]);
        static getPhone(phoneType) {
            const factory = mobilePhoneFactory.phoneMaps.get(phoneType);
            return new factory();
        }
    }

    const android1 = mobilePhoneFactory.getPhone('android');
    const ios1 = mobilePhoneFactory.getPhone('ios');
    android1.call();   // android call
    ios1.call();    // ios call
```
#### 场景
* 暴露单个方法给用户，根据不同参数实例化不同对象

### 工厂方法模式
#### 每个对象类都有自己的工厂类
#### 示例：

```javascript
    interface IPhone {
        call(): void;
    }
    interface IPhoneFactory {
        getPhone(): IPhone;
    }

    class androidPhone implements IPhone {
        call() {
            console.log('android call');
        }
    }

    class androidPhontFactory implements IPhoneFactory {
        getPhone() {
            return new androidPhone();
        }
    }

    class iosPhone implements IPhone {
        call() {
            console.log('ios call');
        }
    }
     class iosPhontFactory implements IPhoneFactory {
        getPhone() {
            return new iosPhone();
        }
    }
    
    const androidFacroty = new androidPhontFactory();
    const android1 = androidFacroty.getPhone();
    android1.call();   // android call
```
#### 场景
* 用户可以根据不同的工厂函数创建不同的对象

### 抽象工厂模式
#### 相对于工厂方法的不同，需要多个对象完成某个功能
#### 示例：

```javascript
    interface IUi {
        createUi(): void;
    }
    interface IOperation {
        createOperation(): void;
    }

    interface IPhoneFactory {
        getOperation(): IOperation;
        getUi(): IUi;
    }

    class androidUi implements IUi {
        createUi() {
            console.log('android ui');
        }
    }
    class androidOperation implements IOperation {
        createOperation() {
            console.log('android operation');
        }
    }
    class androidFactory implements IPhoneFactory {
        getOperation() {
            return new androidOperation();
        }
        getUi() {
            return new androidUi();
        }
    }

    class iosUi implements IUi {
        createUi() {
            console.log('ios ui');
        }
    }
     class iosOperation implements IOperation {
        createOperation() {
            console.log('ios operation');
        }
    }

    class iosFactory implements IPhoneFactory {
        getOperation() {
            return new iosOperation();
        }
        getUi() {
            return new iosUi();
        }
    }

   
    const androidFacroty = new androidFactory();
    const androidUi = androidFacroty.getUi();
    androidUi.createUi()   // android ui
    const androidOperator = androidFacroty.getOperation();
    androidOperator.createOperation() // android operation
```

#### 场景
* 用户可以根据不同的工厂函数创建不同的对象
* 一个工厂对象的功能需要几个对象来共同完成

### 参考

- <a style='color:#0A497B' href='https://www.jianshu.com/p/83ef48ce635b' target='_blank'>工厂模式</a>
