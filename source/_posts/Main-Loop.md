---
title: Main Loop
date: 2016-08-17 18:37:10
categories: QToolKit
comments: true
tags: [MainLoop,QToolKit]
keywords: MainLoop,QToolKit
---

[QTK](https://github.com/qtoolkit/qtk)中的Main Loop主要是管理QTK的渲染循环，与直接调用用requestAnimationFrame相比，它有下列好处：

1.多次请求重绘没有副作用。在QTK中每个控件的属性有变化时都需要重绘，控件并不需要知道别的控件有没有请求重绘，只管自己有变化时请求重绘就行了，这样势必出现重复请求重绘，MainLoop只在第一次请求重绘时才调用requestAnimationFrame，重复的请求只是增加一下计数。


2.把绘制分成绘制前，绘制和绘制后多个阶段。动画可以在绘制前这个阶段执行，这样参数的变化就能在本次绘制中体现出来，而一些不太重要的事情可以放到绘制后执行。

3.节省能源，绿色环保。控件有变化时请求重绘，没有变化时不需要重绘，只在有执行动画时才跑到最大帧率，而不是像游戏一样一直保持60FPS。

4.注册的回调函数会一直执行，直到注销为止。

使用方法如下（javascript）:

```
function draw(evt) {
	if(quit) {
		qtk.MainLoop.off(qtk.MainLoop.DRAW, draw);
	}
}
qtk.MainLoop.on(qtk.MainLoop.DRAW, draw);

```
