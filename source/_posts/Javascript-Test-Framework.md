---
title: Javascript Test Framework
date: 2016-08-06 21:23:54
categories: 学习笔记
comments: true
tags: [Test,Karma,Mocha]
keywords: QToolKit,Test,Karma,Mocha
---

我决定从开始就对[QToolKit](https://github.com/qtoolkit)相关项目进行严格测试。由于对测试程序框架不熟悉，在选择测试程序框架时遇到一些问题。最初打算使用[jest](https://github.com/facebook/jest)，倒不是看中它有强大自动mock功能，而是相信facebook自己使用的东西错不了。但是我在测试程序中使用XMLHTTPRequest时，总是提示网络不可用。在网上找到的答案都是关于如何mock XMLHTTPRequest的，而我更想进行实际的网络请求测试。所以只好重新审视自己的需求，希望找一个真正合适的测试程序框架：

1.能够在真实浏览器中测试。除了[karma](https://github.com/karma-runner/karma)，好像没有什么可以选择。这货功能确实非常强大：

```
*.You want to test code in real browsers.
*.You want to test code in multiple browsers (desktop, mobile, tablets, etc.).
*.You want to execute your tests locally during development.
*.You want to execute your tests on a continuous integration server.
*.You want to execute your tests on every save.
*.You love your terminal.
*.You don't want your (testing) life to suck.
*.You want to use Istanbul to automagically generate coverage reports.
*.You want to use RequireJS for your source files.
```

2.使用起来方便，支持同步和异步功能的测试。karma不是测试程序框架也不是assert库，还需要自己选择相应的插件。我开始选择的是[jasmine](https://github.com/jasmine/jasmine)，用jasmine的人很多，上手也很简单。用了两天后，发现没法测试异步函数(后来找到了相应的插件)，又去研究了人气很高的[mocha](https://github.com/mochajs/mocha)，它对异步测试支持非常完美。果断选择了mocha， mocha也没有提供assert库，由使用者自己选择第三方assert库。我选择了[better-assert](https://github.com/tj/better-assert)，主要原因是它简单，而且合乎我的习惯。


3.能提供代码覆盖率的报表。karma也有converage相应的插件。使用起来比较方便。当然也有些小问题，一是源代码会被改得面目全非，如果有问题没法调试，所以调试版本需要关掉代码覆盖率的测试功能。二是第三方库也会被统计起来，而这些库通常没有专门的测试程序，这会拉低整体的覆盖率。为了让报表好看一点，也可以为这些库写一些测试程序。

4.方便集成github持续集成的功能。karma也支持第三方的持续集成系统，其中不少支持github，这一部分看起来比较简单，用到的时候再深入研究。

[QToolKit](https://github.com/qtoolkit)是用Typescript开发的，先用tsc编译成JS，然后用webpack打包成一个文件，再使用上面的框架，整个过程还是比较流畅的：

1.先安装node及npm，为了避免不必要的麻烦，请安装最新的版本。

2.创建一个项目。

```
mkdir foo
cd foo
npm init

//test command输入下面的内容： 
//tsc && webpack && ./node_modules/karma/bin/karma start

```

3.安装依赖的软件包，软件包比较多，建议使用[淘宝的镜像](https://npm.taobao.org/)。

```
npm install karma karma-chrome-launcher karma-coverage karma-firefox-launcher karma-mocha mocha ts-loader typescript typings webpack --save
```

4.初始化Typescript项目，生成tsconfig.json。

```
tsc --init
```

5.编写代码。

src/sum.ts

```
export = function sum(x:number, y:number) : number {
    return x + y;
}
```

src/index.ts

```
export import sum = require("./sum");
```

__tests__/sum.test.js
```
describe('sum', function() {
  it('test sum', () => {
    assert(foo.sum(100, 200) === 300);;
  });
});
```

6.创建一个webpack的配置文件webpack.config.js。

```
var webpack = require('webpack');
var libraryName = 'foo';

module.exports = {
  cache: true,
  entry: {
    index : './src/index.js'
  },
  output: {
    path: '',
    filename: '[name].js',
    library: libraryName
  },
  resolve: {
    extensions: ['', '.webpack.js', '.web.js', '.js']
  }
}
```

7.创建karma的配置文件，先运行下面的命令，再手工修改：

```
./node_modules/karma/bin/karma init
```

加入测试需要的JS文件：

```
    files: [
      'index.js',
      '__tests__/assert.js',
      '__tests__/*.test.js'
    ],

```

增加覆盖率支持：

```
    preprocessors: {
        'index.js': ['coverage']
    },

    reporters: ['progress', 'coverage'],
    
    overageReporter: {
      type : 'html',
      dir : 'coverage/'
    },
```

基本功能完成了，运行测试:

```
npm run test
```

这时会自动启动浏览器进行测试，浏览器中有个Debug按钮，点击Debug按钮可以打开被测试的页面，如果有问题可以在浏览器中调试。覆盖率的数据在coverage目录下，这个是在karma的配置文件中指定的。

里面的配置参数很多，等用到了再去研究。

参考：[Foo](https://github.com/qtoolkit/foo)


