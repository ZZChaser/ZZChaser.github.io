---
layout: post
title: '按需加载js'
date: 2019-05-25
author: ZZChaser
# cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: js
---

> 按需加载 js

#### 在angular中

```javascript
    import { Injectable, Inject } from '@angular/core';
    import { ReplaySubject, Observable } from 'rxjs';
    import { DOCUMENT } from '@angular/common';

    @Injectable()
    export class LazyLoadingScriptService {
        _loadedLibraries: { [url: string]: ReplaySubject<any> } = {};

        constructor(@Inject(DOCUMENT) private readonly document: any) { }

        loadScript(url: string): Observable<any> {
            if (this._loadedLibraries[url]) {
                return this._loadedLibraries[url].asObservable();
            }
            this._loadedLibraries[url] = new ReplaySubject();
            const script = this.document.createElement('script');
            script.type = 'text/javascript';
            script.src = url;
            script.onload = () => {
                this._loadedLibraries[url].next();
                this._loadedLibraries[url].complete();
            };
            this.document.body.appendChild(script);
            return this._loadedLibraries[url].asObservable();
        }
    }

    /* Usage */
    this.lazyLoadService.loadScript('/assets/scripts/some-script.js').subscribe(() => {
    // code
    });
```

#### 通用方法

```javascript
    
    scirpt = [];
    loadScript(url: string): Promise<any> {
        return new Promise((resolve) => {
             if (scirpt.indexOf(url) > -1) {
                resolve();
            } else {
                scirpt.push(url);
                const script = document.createElement('script');
                script.type = 'text/javascript';
                script.src = url;
                script.onload = () => {
                    resolve()
                };
                this.document.body.appendChild(script);
            }
        })
    }

    /* Usage */
    loadScript('/assets/scripts/some-script.js').then(() => {
    // code
    });

   
```
### 参考

-   <a style='color:#0A497B' href='https://netbasal.com/loading-external-libraries-on-demand-in-angular-9dad45701801' target='_blank'>Loading External Libraries on Demand in Angular</a>

