---
layout: post
title: 'angular optimization'
date: 2018-12-29
author: ZZChaser
cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: angular
---

> angular 中的优化

### Pipe
&emsp;&emsp;Pipe是用来转换绑定值，默认的纯管道，只有在绑定值发生纯变动才会执行，所以会跳过一些不必要的变更检查。
### TrackBy
&emsp;&emsp;在ngFor循环中，使用trackBy来告诉angular怎样跟踪集合的各项（类似于react中的唯一key），最后优化渲染。trackBy函数接受两个参数，当前序号和当前项，并返回唯一标识

```javascript
  import{ Component } from '@angular/core';
  
  @Component({
    selector: 'trackBy-test',
    template: `
      <ul><li *ngFor="let item of items; trackBy: trackByIndex">{{item.name}}</li></ul>
      <button (click)="getItems()">Get Items</button>
    `
  })
  export class TrackByCmp{
    items: any[]=[];
    constructor(){
      this.items = [{name:'Tom'},{name:'Jerry'},{name:'Kitty'}];
    }
    getItems(){
      this.items = [{name:'Tom'},{name:'Jerry'},{name:'Mac'},{name:'John'}];
    }
    trackByIndex(index, item){
      return index;
    }
  }
    
```

### 组件使用OnPush策略 + ChangeDetectorRef 
&emsp;&emsp;在组件的元数据中添加changeDetection:ChangeDetectionStrategy.OnPush，表示使用OnPush策略。表示只有@Input输入属性发生纯变化时，才会触发变更检测。
```javascript
  import { Component, Input, OnInit, ChangeDetectionStrategy} from '@angular/core';

  @Component(
    {    
      selector: 'exe-counter',    
      template: `      <p>当前值: {{ counter }}</p>    `,    
      changeDetection: ChangeDetectionStrategy.OnPush 
    }
  )
```

&emsp;&emsp;ChangeDetectorRef是组件变化检测器的引用，主要有以下几种方法：
1. markForCheck()：在组件的 metadata 中如果设置了 changeDetection: ChangeDetectionStrategy.OnPush 条件，那么变化检测不会再次执行，除非手动调 用该方法。该方法使当前元素及所有祖先元素直到根元素的变更检测都开启。
2. detach()：从变化检测树中分离变化检测器，该组件的变化检测器将不再执行变化检测，除非手动调用 reattach() 方法。
3. reattach()：重新添加已分离的变化检测器，使得该组件及其子组件都能执行变化检测。
4. detectChanges()：从该组件到各个子组件执行一次变化检测。



### Run outside Angular
&emsp;&emsp;通过注册NgZone服务，在其runOutsideAngular()方法中执行，angular就不会对其变化进行跟踪。如果某个时刻又想让angular进行跟踪变化，就调用run()方法
```javascript
  import { Component, OnInit, NgZone } from '@angular/core';

  @Component({
    selector: 'app-dashboard',
    templateUrl: './dashboard.component.html',
    styleUrls: ['./dashboard.component.css']
  })
  export class DashboardComponent implements OnInit {
    progress: number = 0;
    label: string;
    constructor(private _ngZone: NgZone) { }

    ngOnInit() { }
    processWithinAngularZone() {
      this.label = 'inside';
      this.progress = 0;
      this._increaseProgress(() => console.log('Inside Done!'));
    }

    ngDoCheck(): void {
      //Called every time that the input properties of a component or a directive are checked. Use it to extend change detection by performing a custom check.
      //Add 'implements DoCheck' to the class.
      console.log('check')
    }
    processOutsideOfAngularZone() {
      this.label = 'outside';
      this.progress = 0;
      this._ngZone.runOutsideAngular(() => {
        this._increaseProgress(() => {
          // reenter the Angular zone and display done
          this._ngZone.run(() => { console.log('Outside Done!') });
        })
      });

    }

    _increaseProgress(doneCallback: () => void) {
      this.progress += 1;
      // console.log(`Current progress: ${this.progress}%`);
      if (this.progress < 100) {
        window.setTimeout(() => this._increaseProgress(doneCallback), 10)
      } else {
        doneCallback();
      }
    }
  }

```

