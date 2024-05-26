+++
title = "PDF 文件结构-image (五) "
description = "PDF 文件结构"
date = 2024-05-26
updated = 2024-05-26
draft = false

[taxonomies]
tags = ["PDF"]
[extra]
math = true
math_auto_render = true
keywords = "PDF"
toc = true
series = "PDF"
+++

[上章](https://rockyzhengwu.github.io/pdf-graphics/)总的介绍了 Graphics, Image 在 PDF 中有两种，一种是作为 External Objects 出现，External Objects 就是一个独立于 Content Stream 的 一个 PDF Stream 的对象, 也叫做 XObjects 。
另一种是 inline Image , Image 的内容直接在 Content Stream 中出现。

## External Image
所有的 External Objects 都是通过 `Do` 这个 Graphics Operation 绘制在页面上的，Extrenal Image 也不例外。下面就是一个绘制 Image 的例子,其中 `/Image1` External Objects 中的名字，拿着这个名字可以去当前页面的 Resource Dictionary 中的 `XObjects` 下面获取到 `/Image1` 在 PDF 文档中的具体位置和内容.
```
/Image1 Do
```
Image 在 PDF 中可以看成一个采样的数组，每个元素是一个颜色.
Image 是作为 PDF Stream 存在的，Stream 的 Dictionary 部分存放很多 Image 的属性，
需要把 Image 绘制出来需要知道一些信息
图像的格式: 宽，高，颜色空间的颜色有几个元素，每个元素占用的 Bit 位是多少，比如 DeviceGray 这个颜色就只有一个颜色，8 个 bit 就可以表示一个颜色(256 阶灰度)。 图像应该绘制在页面的什么位置。

## Inline Image

Image Image 是用几个命令一起使用来定义的。 `BI` 是 Begin Image, 后面存储的是 Image 的属性，ID 后面是 Image Data ， 里面存储的是图像的内容。
```
BI
… Key-value pairs …
ID
… Image data …
EI
```

## ColorSpace

图像中一个复杂的地方是把图像内容转化成对应的颜色，PDF 的颜色空间(Color Space) 有三种大的类型
- Device: DreviceGray, DeviceRGB, DeviceCMYK
- CIE-Based: CalGray，CalRGB， Lab, ICCBased
- Special: Indexed, Pattern, Separation,DeviceN
