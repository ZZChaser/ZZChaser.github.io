---
layout: post
title: 'dynamic component in Angular'
date: 2019-10-22
author: ZZChaser
tags: angular
---

> Angular 中动态创建 Component

### 使用 Compiler

```javascript
import {
    Component,
    ViewChild, ViewContainerRef, ComponentRef,
    Compiler, ComponentFactory, NgModule, ModuleWithComponentFactories, ComponentFactoryResolver
} from '@angular/core';

import { CommonModule } from '@angular/common';

@Component({
    selector: 'runtime-content',
    template: `
        <div #container></div>
    `
})
export class DynamicComponent {

    @ViewChild('container', { read: ViewContainerRef })
    container: ViewContainerRef;
    private componentRef: ComponentRef<{}>;

    constructor(
        private componentFactoryResolver: ComponentFactoryResolver,
        private compiler: Compiler) {
    }

    compileTemplate() {
        let metadata = {
            selector: `dynamic-component`,
            template: this.template
        };

        let factory = this.createComponentFactorySync(this.compiler, metadata);

        if (this.componentRef) {
            this.componentRef.destroy();
            this.componentRef = null;
        }
        this.componentRef = this.container.createComponent(factory);
    }

    private createComponentFactorySync(compiler: Compiler, metadata: Component): ComponentFactory<any> {
        const cmpClass = class RuntimeComponent { name: string = 'test' };
        const decoratedCmp = Component(metadata)(cmpClass);

        @NgModule({ imports: [CommonModule], declarations: [decoratedCmp] })
        class RuntimeComponentModule { }

        let module: ModuleWithComponentFactories<any> = compiler.compileModuleAndAllComponentsSync(RuntimeComponentModule);
        return module.componentFactories.find(f => f.componentType === decoratedCmp);
    }

}
```

### 优缺点
* 可以动态根据html和style生成组件
* build不能使用aot模式，导致包体变大。
* 使用运行时编译，性能受影响

### 参考

-   <a style='color:#0A497B' href='https://medium.com/@DenysVuika/dynamic-content-in-angular-2-3c85023d9c36' target='_blank'>Dynamic content in Angular</a>
