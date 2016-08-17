---
title: Assets Manager
date: 2016-08-07 21:19:47
categories: QToolKit
comments: true
tags: [Assets,QToolKit]
keywords: Assets,QToolKit
---

[QTK](https://github.com/qtoolkit/qtk)中的Assets主要是对APP使用的资源进行管理，资源包括图片、声音、字体和其它数据。

1.加载指定的资源。加载指定URL的资源，返回一个Promise。如果资源已经加载，就直接从缓存中获取，如果没有加载，就从网络获取，并放入到缓存中。

```
var asset = qtk.Assets.loadJSON("http://localhost:9876/base/www/not_exist.txt")
    .then(json => {

     }, err => {

     });
```

2.预先加载资源。通常在APP启动时，预先加载某些资源。把这些资源放到一个分组中，分组中每加载完成一个资源，就会触发加载进度事件，界面可以根据加载进度事件更新进度条，整个分组加载完成后才启动APP。

```
var items = [
            {type:qtk.Assets.TEXT, src:"http://localhost:9876/base/www/test.txt"},
            {type:qtk.Assets.JSON, src:"http://localhost:9876/base/www/test.json"},
            {type:qtk.Assets.IMAGE, src:"http://localhost:9876/base/www/test.jpg"},
            {type:qtk.Assets.BLOB, src:"http://localhost:9876/base/www/test.blob"}
];
var assets = new qtk.Assets.Group(items);
assets.onProgress(function(info) {
    if(info.total === info.loaded && info.loaded === items.length) {
         done();
    }
    console.log(JSON.stringify(info));
});
```

3.清除缓存中的资源。一些资源在不需要时，可以从缓存中清除，以释放内存。

```
qtk.Assets.clear("http://localhost:9876/base/www/test.jpg");
```



