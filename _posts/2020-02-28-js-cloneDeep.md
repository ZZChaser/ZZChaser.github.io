---
layout: post
title: 'js的深拷贝'
date: 2020-02-28
author: ZZChaser
# cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: js
---

> js深拷贝的简单整理

### 浅拷贝与深拷贝
* 浅拷贝：js的数据类型中分为值类型和引用类型，浅拷贝在复制引用类型的时候只复制一层，拷贝地址，指向了同一份数据。
* 深拷贝：深拷贝是进行无限层次的拷贝

#### 说明示例
```javascript
    const oriObj = {a:{a1:2}};
    const shallowObj = cloneShallow(oriObj);  // 浅拷贝
    const deepObj = cloneDeep(oriObj);      //深拷贝
    console.log(oriObj.a === shallowObj.a); // => true
    console.log(oriObj.a === deepObj.a); // => false
```
### 浅拷贝
```javascript
    function cloneShallow(data) {
        if(typeof data !== "object") return data;
        let copyData = {};
        if(Array.isArray(data)) {
            copyData = [];
        }
        for(const key in data) {
            // for in 会遍历自身和原型链上的可枚举属性，使用hasOwnProperty过滤掉原型上的属性
            if (data.hasOwnProperty(key)) {
                copyData[key] = data[key];
            }
        }
        return copyData;
    }
```
### 深拷贝
```javascript
    function cloneDeep(data) {
        if(typeof data !== "object") return data;
        let copyData = {};
        if(Array.isArray(data)) {
            copyData = [];
        }
        for(const key of Object.keys(data)) {
            if (typeof data[key] !== "object") {
                copyData[key] = cloneDeep(data[key]);
            }else {
                copyData[key] = data[key];
            }
        }
        return copyData;
    }
```
&emsp;深拷贝中潜在的问题
* 循环引用导致爆栈
* 引用丢失
* 递归层次过深还有爆栈可能

#### 解决循环引用
&emsp;对象中的一个子项与该对象或者该对象的父级的引用相同，说明存在循环引用
```javascript
    const obj = {};
    obj.a = obj;
```
&emsp;使用循环检测，如果父级中已有该对象，则直接返回
```javascript
    function cloneDeep(data, parent = null) {
        if(typeof data !== "object") return data;
        let copyData = {};
        while(parent) {
            if (parent.oriParent === data) {
                return parent.oriParent;
            }
            parent = parent.parent
        }
         if(Array.isArray(data)) {
            copyData = [];
        }
        for(const key of Object.keys(data)) {
            if(typeof data[key] !== "object") {
                copyData[key] = data[key];
            } else {
                copyData[key] = cloneDeep(data[key], {
                    oriParent: data,
                    parent
                });
            }
        }
        return copyData;
    }
```
#### 解决引用丢失
&emsp;对象中不同的项，有相同的引用
```javascript
    const singleObj = {a:5}
    const obj = {a:singleObj, b:singleObj};
    console.log(obj.a ===obj.b);  //=> true
    const copyObj = cloneDeep(obj);
    console.log(copyObj.a ===copyObj.b); //=> false
```
&emsp;使用map记录原对象和copy对象的对应关系，如果要复制的对象已存在，则直接返回该对象对应的copy对象
```javascript
    function cloneDeep(data) {
        if(typeof data !== "object") return data;
        const clonedMap = new Map();
        function clone(data) {
            if(typeof data !== "object") return data;
            let copyData = {};
            const mapItem = clonedMap.get(data);
            if (mapItem) {
                return mapItem;
            }
            clonedMap.set(data, copyData);

            if(Array.isArray(data)) {
                copyData = [];
            }
            for(const key of Object.keys(data)) {
                if(typeof data[key] !== "object") {
                    copyData[key] = data[key];
                } else {
                    copyData[key] = clone(data[key]);
                }
            }
            return copyData;
        }
        return clone(data);
    }
```
#### 解决递归
&emsp;使用循环代替递归
```javascript
    function cloneDeep(data) {
        if(typeof data !== "object") return data;
        const root = {};
        const tree = [{
            parent: root,
            key: '',
            data
        }];
        while (tree.length > 0) {
            const item = tree.pop();
            const { parent, key, data } = item;
            let res = parent;
            if (key) {
                res = parent[key] = {};
            } 
            for (const key of Object.keys(data)) {
                const dataItem = data[key];
                if (typeof dataItem === "object") {
                    const node = {
                        parent: res,
                        key,
                        data: dataItem
                    }
                    tree.push(node);
                } else {
                    res[key] = dataItem;
                }
            }
        }
        return root;
    }
    
```
### 说明
&emsp;以上示例只是说明深拷贝中的这些典型问题，忽略了类型的具体判断和不同的处理
### 其它
#### 最简单的深拷贝
```javascript
    function cloneDeep(data) {
        return JSON.parse(JSON.stringify(data))
    }
```
该方法存在以下问题存在的问题
* 当data中存在function，RegExp，Map，Set等数据格式不会正确拷贝
* 当存在循环引用时会出错
#### 深拷贝一个函数
```javascript
    function sayName(name) {
        console.log(`name is ${name}`);
    }
    const copySayName = eval(`(${sayName.toString()})`);
    copySayName('ok')  //=> "name is ok"
```
### 参考
* <a style='color:#0A497B' href='https://segmentfault.com/a/1190000016672263' target='_blank'>深拷贝的终极探索</a>
* <a style='color:#0A497B' href='https://zhuanlan.zhihu.com/p/23251162' target='_blank'>Javascript之深拷贝</a>

