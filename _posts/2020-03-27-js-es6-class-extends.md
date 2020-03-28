---
layout: post
title: 'ES6 class extends'
date: 2020-03-27
author: ZZChaser
# cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: ES6
---

> ES6使用`extends`来实现继承，相对于ES5修改原型链，更方便和清晰

### 基本语法
```javascript
    class Sup {
        constructor(name, age) {
            this.name = name;
            this.age = age;
        }
        sayName() {
            console.log(this.name);
        }
    }
    class Sub extends Sup {
        constructor(name, age, career) {
            super(name, age);
            this.career = career;
        }
    }
    const test1 = new Sub('张三',18,'程序员');
    test1.sayName();  // 张三  
```
* 子类的构造函数中必须调用父类的构造函数`super()`
* 子类的`this`必须在`super()`后使用，子类的实例是基于父类的实例进行构建
* 当没有显示写`constructor`函数时，会自动加上

### `super`关键字
1. 用作函数（只能在构造函数中使用）：表示父类的构造函数，
```javascript
    class Sub extends Sup {
        constructor(name, age, career) {
            super(name, age);  // 相当于 Sup.prototype.constructor.apply(this,[ name, age]);
            this.career = career;
        }
    }
```
2. 用作对象（普通方法中表示父类原型对象，静态方法中表示父类）,其中的`this`均指向子类
```javascript
    class Sup {
        static flag = 'Sup';
        static sayFlag() {
            console.log(this.flag);
        }
        constructor(name, age) {
            this.name = name;
            this.age = age;
        }
        sayName() {
            console.log(this.name);
        }
    }
    
    class Sub extends Sup {
        static flag = 'Sub';
        static sayFlag() {
            super.sayFlag();
        }
        constructor(name, age, career) {
            super(name, age);
            this.career = career;
        }
        sayName() {
            super.sayName();
        }
    }
    const test1 = new Sub('张三',18,'程序员');
    test1.sayName();  // 张三
    Sub.sayFlag();  // Sub
```

### 类的`__proto__`和`prototype`
&emsp;&emsp;ES6继承按如下模式实现
```javascript
    class Sup {
    
    }
    class Sub {
      
    }
    Object.setPrototypeOf(Sub.prototype, Sup.prototype);
    Object.setPrototypeOf(Sub, Sup);

    // setPrototypeOf源码为
    Object.setPrototypeOf = function(obj, proto) {
        obj.__proto__ = proto;
        return obj;
    }
```
1. 子类的`__proto__`表示构造函数的继承，指向父类。理解为子类的原型是父类
2. 子类的`prototype.__proto__`表示方法的继承，指向父类的`prototype`。理解为子类构造函数的原型是父类的实例。相当于ES5中寄生组合继承`Sub.prototype = Object.creat(Sup.prototype)`

### 实例的`__proto__`
&emsp;&emsp;子类实例的`__proto__`的`__proto__`指向父类的原型
```javascript
    class Sup {
    
    }
    class Sub extends Sup{
      
    }
    const test1 = new Sub();
    test1.__proto__.__proto__ === Sup.prototype;  // true
```
### 原生构造函数的继承（Object, Array, Error, Function, Number...）
&emsp;&emsp;在ES5中原生构造函数是不能被继承的
```javascript
    function MyArr() {
        Array.apply(this, arguments);
    }
    MyArr.prototype = Object.create(Array.prototype, {
    constructor: {
        value: MyArr,
        writable: true,
        configurable: true,
        enumerable: true
    }})
    var arr = new MyArr(1,2,3);
    arr[5] = 6;
    console.log(arr);  // MaArr{5: 6}
    console.log(arr.length);  // 0
```
这是因为子类无法获取到原生构造函数的内部属性。原生构造函数会忽略`this`，导致无法绑定`this`，拿不到内部属性。
```javascript
    class MyArr extends Array {
    }
    var arr = new MyArr(1,2,3);
    arr[5] = 6;
    console.log(arr);  //MyArr(6) [1, 2, 3, empty × 2, 6]
    console.log(arr.length);  // 6
```
* ES5：先创建子类的实例`this`，再将父类的属性添加到`this`上
* ES6：先创建父类的实例`this`，再执行子类的构造函数添加属性
* ES5子类不会继承父类的静态属性和静态方法

### 参考
* <a style='color:#0A497B' href='https://es6.ruanyifeng.com/#docs/class-extends' target='_blank'>Class 的继承</a>

