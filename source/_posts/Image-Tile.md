---
title: Image Tile
date: 2016-08-16 12:19:04
categories: QToolKit
comments: true
tags: [ImageTile,QToolKit]
keywords: ImageTile,QToolKit
---

[QTK](https://github.com/qtoolkit/qtk)中的ImageTile表示一个图块，它可以是一整张图片，也可以是图片中的一块区域。x和y表示图块的位置，w和h表示图块的宽度和高度。

ImageTile的主要功能有两个：

#### 加载指定URL的图片。

URL通常包括两个部分，用#分隔，前面部分是大图的信息，后面是图块的信息，如果没有#，则表示整张图片。目前支持4种格式的URL：

1.普通图片URL。此时ImageTile表示整张图片。如：

```
http://www.example.com/image.png
```

2.普通图片URL+'#'+x{x}y{y}w{w}h{h}，它表示指定位置(xy)和大小(wh)的子图。如：

```
http://www.example.com/image.png#x10y10w100h100
```

3.普通图片URL+'#'+r{行数}c{列数}i{索引}，它表示把图片分成指定行数和列数等大小的图块，取其中第i块，i的取值范围为0到r*c。如：

```
http://www.example.com/image.png#r3c3i0
```

4.JSON文件URL+'#'+小图的名称，它表示TexturePacker生成的JSON Hash格式文件中的一个小图。如：


```
http://www.example.com/images.json#0.png
```

#### 将图块绘制在指定的HTML5 Canvas上。

ImageTile实现了多种绘制的方式，使用时指定绘制方式和目标区域的位置和大小，目前支持的绘制方式有：

1.缺省方式，将图块填满指定的区域。

2.居中方式，将图块按原大小绘制到指定区域的中间。

3.自动方式，将图块缩放显示到指定区域，保持图片的宽高不变。

4.特殊九宫格方式，将图块分成等大小的九块，按九宫格的方式显示。

5.水平三宫格方式，将图块按水平方向分成等大小的三块，左右两块分别显示在目标区域的左右两边，中间的一块缩放显示在目标区域的中间剩余部分。

6.垂直三宫格方式，将图块按垂直方向分成等大小的三块，上下两块分别显示在目标区域的上下两边，中间的一块缩放显示在目标区域的中间剩余部分。

7.水平平铺。水平方向平铺显示，垂直方向拉伸，如果想保证垂直方向不拉伸，请指定图块同样的高度。

8.垂直平铺。垂直方向平铺显示，水平方向拉伸，如果想保水平方向不拉伸，请指定图块同样的宽度。

9.平铺。水平和垂直方向均平铺显示。





