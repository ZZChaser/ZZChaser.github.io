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
#### 简单示例：
```javascript
    function cloneShallow(data) {
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

    function cloneDeep(data) {
        if (typeof data !== "object") {
            return data;
        }
        let copyData = {};
        if(Array.isArray(data)) {
            copyData = [];
        }
        for(const key of Object.keys(data)) {
            if(typeof data[key] !== "object") {
                return data[key];
            } else {
                return cloneDeep(data[key]);
            }
        }
    }
```
### 深拷贝中的问题
* 数据类型判断不精确
* 数据层次多容易栈溢出
* 循环引用问题
#### 添加数据类型的判断
```javascript
    function cloneDeep(data) {
        // 判断基础类型，除了null
        if (typeof data !== "object") {
            return data;
        }
        // 单独处理null
        if (data === null) {
            return data;
        }

        let copyData = {};

        // 处理Set
        if (Object.prototype.toString.call(data) === "[object Set]") {
            return new Set([...data]);
        }
        // Map Function

        // 判断数组
        if(Array.isArray(data)) {
            copyData = [];
        }
        // 
        for(const key of Object.keys(data)) {
            if(typeof data[key] !== "object") {
                return data[key];
            } else {
                return cloneDeep(data[key]);
            }
        }
    }
```
#### 使用循环代替递归
```javascript
    // 大概思路
    function cloneDeep(data) {
        const root = {};
        const treeNode = [{
            parent: root,
            key: undefined,
            data: data
        }];
        while(treeNode.length > 0) {
            cosnt node = treeNode.pop();
            const { parent, key, data } = node;
            let res = parent;
            if (key === undefined) {
                res = parent[key] = {};
            }
            for (const key of Object.keys(data)) {
                if (typeof data[key] !== "object") {
                    res[key] = data[key];
                } else {
                    const nextNode = {
                        parent: res,
                        key,
                        data: data[key],
                    };
                    treeNode.push(nextNode);
                }
            }
        }
        return root;
    }
```
#### 记录复制过的object
```javascript
    // 大概思路
    function cloneDeep(data) {
        const root = {};
        const uniqueList = [];
        const treeNode = [{
            parent: root,
            key: undefined,
            data: data
        }];
        while(treeNode.length > 0) {
            const node = treeNode.pop();
            const { parent, key, data } = node;
            let res = parent;
            if (key === undefined) {
                res = parent[key] = {};
            }

            const uniqueData = find(uniqueList, data);
            if (uniqueData) {
                res = uniqueData.target
                continue;
            }

            uniqueList.push({
                source: data,
                target: res,
            });

            for (const key of Object.keys(data)) {
                if (typeof data[key] !== "object") {
                    res[key] = data[key];
                } else {
                    const nextNode = {
                        parent: res,
                        key,
                        data: data[key],
                    };
                    treeNode.push(nextNode);
                }
            }
        }
        return root;
    }
```
### 其它
* JSON.parse(JSON.stringify())
* copy function eval(fun.toString())
### 参考
* <a style='color:#0A497B' href='https://segmentfault.com/a/1190000016672263' target='_blank'>深拷贝的终极探索</a>

