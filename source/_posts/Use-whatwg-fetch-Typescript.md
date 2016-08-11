---
title: Use whatwg-fetch Typescript
date: 2016-07-31 12:16:18
categories: 学习笔记
comments: true
tags: [Typescript,whatwg-fetch]
keywords: whatwg-fetch,Typescript
---

说来惭愧，最近才知道的[whatwg-fetch](https://fetch.spec.whatwg.org/)的这个标准，firefox和chrome已经支持它了，github实现了相应的[polyfill](https://github.com/github/fetch.git)，typings里也有相应的定义，所以决定在[qtk](https://github.com/qtoolkit/qtk)里直接使用，而不是再去包装XMLHttpRequest。


#### 安装JS包

```
cnpm install es6-promise --save
cnpm install whatwg-fetch --save
```

#### 安装typings
```
typings install dt~es6-promise --save --global
typings install dt~whatwg-fetch --save --global
```

#### 引入相应的包
```
import Promise = require("es6-promise");
import fetch = require("whatwg-fetch");
```

却奇怪的是出现错误：
```
error TS2307: Cannot find module 'whatwg-fetch'.
```

查了一些资料才知道，这种polyfill，只是修改了一些全局的状态，并没有导出什么东西的包，应该用下面的方式导入：

```
import 'whatwg-fetch';
```

还是typescript不熟悉啊，得花点时间系统的学习一下。

参考：[https://www.typescriptlang.org/docs/handbook/modules.html](https://www.typescriptlang.org/docs/handbook/modules.html#import-a-module-for-side-effects-only)
