---
layout: post
title: 'angular添加http拦截-HttpInterceptor'
date: 2017-04-18
author: ZZChaser
cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: angular
---

> Transform your plain text into static websites and blogs.

### 目的

    为所有请求添加http拦截，统一做出错处理

### 使用

#### 添加service

```
    import { Injectable } from '@angular/core';
    import { HttpEvent, HttpInterceptor, HttpHandler, HttpRequest, HttpResponse, HttpErrorResponse } from "@angular/common/http";
    import { catchError, mergeMap } from 'rxjs/operators';
    import { Observable, of } from 'rxjs';

    @Injectable({
        providedIn: 'root'
    })

    export class InterceptorService implements HttpInterceptor {   //实现HttpInterceptor接口

        constructor() { }

        intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {   //实现intercept方法
            //请求对象和响应对象是不可变的，需要返回克隆的请求对象
            return next.handle(req)
            .pipe(
                mergeMap((event: any) => {
                if (event instanceof HttpResponse && event.status === 200) {
                    return this.handleData(event);  //成功调用处理函数
                } else {
                    return of(event);
                }
                }),
                catchError((err: HttpErrorResponse) => this.handleData(err))  //捕获到错误
            )
        }

        //处理函数
        handleData(event: HttpResponse<any> | HttpErrorResponse): Observable<any> {
            // 业务处理：一些通用操作
            switch (event.status) {
                case 200:
                    if (event instanceof HttpResponse) {
                        //成功处理
                        return of(event);
                    }
                    break;
                case 401: // 未登录状态码
                    break;
                case 404:  //未找到
                case 500:  //服务器出错
                    console.log('系统500---------')
                    break;
                default:
                    return of(event);
            }
        }
    }

```

#### 注入app.module

```
@NgModule({
  declarations: [
    AppComponent,
  ],
  imports: [
    CoreModule,
    BrowserModule,
    BrowserAnimationsModule,
    HttpClientModule,
    AppRoutingModule,
    FormsModule,
  ],
  providers: [
    {
      provide: NZ_I18N,
      useValue: zh_CN
    },
    AppService,
    AuthHttpClient,
    UserBehaviorsApi,
    {
      provide: HTTP_INTERCEPTORS,
      useClass: AuthTokenInterceptor,
      multi: true
    }
  ],
  bootstrap: [AppComponent]
})

export class AppModule {
  constructor() {
  }
}

```

### Helpful HintsPermalink

Throughout this guide there are a number of small-but-handy pieces of information that can make using Jekyll easier, more interesting, and less hazardous. Here’s what to look out for.
