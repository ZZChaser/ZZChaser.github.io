---
layout: post
title: '代理模式'
date: 2020-03-23
author: ZZChaser
# cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: js
---

> 在一些限制下，一个对象不能直接访问另一个对象，需要借助第三方（代理）来达到访问的目的。则称为代理模式

#### 下面已一个vip的视频网站为例

```javascript
    const user = {
        name: 'test1',
        isVip: false
    };
    const allVideos = [{
        videoName: '喜剧之王',
        isVip: true
    }, {
        videoName: '喜剧之王2',
        isVip: false
    }];
    const watchAbleVideo = new Proxy(allVideos, {
        get: (target, propKey) => {
            if (propKey === 'video') {
                if (user.isVip) {
                    return allVideos;
                } else {
                    return allVideos.filter(video => video.isVip === true);
                }
            }
            
        }
    })
   console.log(watchAbleVideo.video);  // [{videoName: '喜剧之王', isVip: false}]
```