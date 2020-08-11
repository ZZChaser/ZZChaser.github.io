---
layout: post
title: '在浏览器中打开exe程序'
date: 2020-03-04
author: ZZChaser
# cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: summary
---

> 在浏览器中调用开启exe程序

### 注册表方式（通用）
#### 添加注册表（先用txt文本编辑，然后修改后缀名为.reg。然后双击运行即可）
```txt
    Windows Registry Editor Version 5.00

    [HKEY_CLASSES_ROOT\zhao]
    @="zhao Protocol"
    "URL Protocol"=""

    [HKEY_CLASSES_ROOT\zhao\DefaultIcon]
    @="C:\\Users\\zhuzhao\\Test.exe"

    [HKEY_CLASSES_ROOT\zhao\shell]
    @=""

    [HKEY_CLASSES_ROOT\zhao\shell\open]
    @=""

    [HKEY_CLASSES_ROOT\zhao\shell\open\command]
    @="\"C:\\Users\\zhuzhao\\Test.exe\" \"%1\" "
```
#### 调用
```html
    <a href="zhao://8080">  open exe  </a>
```
#### 注意事项
* 程序的路径中不能存在中文
* 填写路径时注意斜线的转义
* 传参需要在路径后添加`\"%1\"`，然后命令后面跟的就是参数`8080`
### IE
```javascript
    function startExe(exePath) {
        try{
            //新建一个ActiveXObject对象
            var exe = new ActiveXObject("wscript.shell");
            exe.run(`exePath 8080`); 
        }catch(e) {
            alert('请检查路径是否正确！');
        }
    }
    // 调用
    startExe('C:\\Users\\zhuzhao\\Test.exe')
```
#### 注意事项
* `ActiveXObject`对象只在ie中存在
* 填写路径时注意斜线的转义
* 路径后面空一格再加参数
* 更改IE的安全级别：开始->设置->控制面板->Internet选项->安全->自定义级别->对未标记为安全的ActiveX控件进行初始化和脚本运行->启用。
### 参考
* <a style='color:#0A497B' href='https://blog.csdn.net/longzhoufeng/article/details/78796837?utm_source=blogxgwz3' target='_blank'>打开本地一个exe</a>
* <a style='color:#0A497B' href='https://blog.csdn.net/lmq2582609/article/details/84639207' target='_blank'>使用js打开本地exe应用程序</a>