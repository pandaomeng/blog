---
title: 手机端H5页面适配 踩坑
date: 2018-05-01
tag: [mobile]
---

这两天在公司做手机端H5页面，第一次开发没什么经验，对rem, pt, ppi, dpr这些概念完全不懂，看了很多博客，现总结如下。

#### 对于不同像素宽度的手机，如何做到每一个元素自动缩放？

rem: font size of the root element

rem作为一个单位，单位大小由它第一代老祖宗的**font-size**的大小决定，也就是html标签的**font-size**。例如我们设置html的font-size为10px，那么1.2rem就是12px，我们在所有需要用到px的地方全都使用rem来代替，这样我们只需要改变html的font-size就可以动态地改变内部所有元素的尺寸了。

<!--more-->

**实例**

我们公司提供了宽度为375pt的设计图（pt后面会解释，暂时理解为px就好）。我以这个版本为基础来写页面。我设置html的font-size为10px，那么设计图上的所有关于pt的标注，我只要除以10，再加上rem作为单位就OK了。然后我只要在初始化页面的函数中加上如下代码，就完成了完美的适配。

```
let htmlFontSize = 10 * width / 375 + 'px'
document.getElementsByTagName('html')[0].style.fontSize = htmlFontSize
```

举个栗子：此时手机的宽度变成了414pt，那么根据公式，html的font-size动态地变成了11.04px，页面的其他元素也就跟着放大了。

**PS**：

- 375pt是iphone6/7/8的宽度。

- 414pt是iphone6/7/8 plus的宽度。

  ​


#### pt 和 px 是什么关系呢？

上面我们说到公司里给出的设计图的单位是pt，而我们写页面的时候使用的单位是px，这两者之间有什么联系呢？我们先了解一下逻辑像素和物理像素，以及一些引伸的概念。

1. **逻辑像素pt（点）**

   html css中的使用的单位像素px实际上指的就是逻辑像素pt，一个逻辑像素可能包含多个物理像素（1个，2个或者3个）

2. **物理像素px**

   这个px并不是我们css中用的px，而是一个一个真实的像素点。是photoshop中用的那个px。

3. **dpr**（**device pixel ratio)**

   DPR = 物理像素 / 逻辑像素，iphone 3GS的dpr为1，iphone 4/4s的dpr为2。其他型号的dpr见下表。

4. **ppi（Pixels Per Inch）**

   ppi = 像素数量（pixel） / 显示器的长或宽（inch）

   第一代iphone手机的像素密度是163ppi，但是到了iPhone4的时候像素密度是326ppi (163 * 2)，开发者发现初代的1px(物理像素)和iphone4下的1px(物理像素)显示尺寸(物理尺寸)不相等了，无疑将增加适配的工作量。于是iphone开发者提出了一个pt的概念，即采用初代iphone1个像素点的大小作为基准，记作1pt（point），也就是说1pt在iphone4下的大小=2px的宽高。

   dpr 和 ppi的关系：ppi 和 dpr 成正比

5. **安卓的dp**

   本质上和ios的pt是一样的

   ​

   **实例（iphone机型）**

| **设备****iPhone** | **宽****Width**       | **高****Height**       | **对角线****Diagonal** | **逻辑分辨率(point)** | **Scale Factor(dpr)** | **设备分辨率(pixel)**      | **PPI** |
| ------------------ | --------------------- | ---------------------- | ---------------------- | --------------------- | --------------------- | -------------------------- | ------- |
| 3GS                | 2.4 inches (62.1 mm)  | 4.5 inches (115.5 mm)  | 3.5-inch               | 320x480               | @1x                   | 320x480                    | 163     |
| 4(s)               | 2.31 inches (58.6 mm) | 4.5 inches (115.2 mm)  | 3.5-inch               | 320x480               | @2x                   | 640x960                    | 326     |
| 5c                 | 2.33 inches (59.2 mm) | 4.90 inches (124.4 mm) | 4-inch                 | 320x568               | @2x                   | 640x1136                   | 326     |
| 5(s)               | 2.31 inches (58.6 mm) | 4.87 inches (123.8 mm) | 4-inch                 | 320x568               | @2x                   | 640x1136                   | 326     |
| 6                  | 2.64 inches (67.1 mm) | 5.44 inches (138.3 mm) | 4.7-inch               | 375x667               | @2x                   | 750x1334                   | 326     |
| 6+                 | 3.07 inches (77.9 mm) | 6.23 inches (158.2 mm) | 5.5-inch               | 414x736               | @3x                   | (1242x2208->)**1080x1920** | 401     |





