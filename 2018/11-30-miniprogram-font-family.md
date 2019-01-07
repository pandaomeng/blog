---
title: 微信小程序设置网络字体
date: 2018-11-30
tag: [miniprogram]
---

## 准备工作，获取字体链接

还原设计稿的时候需要用到如下特殊字体（google 的 Montserrat）:

https://fonts.google.com/specimen/Montserrat

1. 选择这个字体

![](https://images.pandaomeng.com/69a7450858cccdc4f6fc354b26917995.jpg)

<!--more-->

2. 下载全部字体

![](https://images.pandaomeng.com/4eecbd05b57012c7cbd9c4ad1a924627.jpg)

3. 将本地的字体文件上传到自己的cdn，（google的源在国内不友好）

   文件解压后如下：
   ![](https://images.pandaomeng.com/f6f997aad808f182ddf2ed49315263ee.jpg)

   选择需要的字体文件上传到cdn上，获得如下链接

   https://images.pandaomeng.com/Montserrat-Regular.ttf

   https://images.pandaomeng.com/Montserrat-Medium.ttf



## 在小程序中使用

1. 官方文档：
   https://developers.weixin.qq.com/miniprogram/dev/api/media/font/wx.loadFontFace.html

2. 封装一个载入字体的util，代码如下

   `utils/loadFont.js`

   ```js
   let loadFont = function (weight = 400) {
     const source = {
       400: 'url("https://images.pandaomeng.com/Montserrat-Regular.ttf")', // Regular
       500: 'url("https://images.pandaomeng.com/Montserrat-Medium.ttf")' // Medium
     }
     wx.loadFontFace({
       family: 'Montserrat',
       source: source[weight],
       desc: {
         weight: weight
       },
       success: function(message) {
         console.log('load font-family success:', message)
       },
       fail: function (message) {
         console.log('load font-family fail: ', message)
       }
     })
   }
   
   export default loadFont
   ```

3. 在需要用到的地方引入

   `pages/xxx/xxx.js`

   ```js
   import loadFont from '../../utils/loadFont'
   
   onLoad () {
       loadFont(400)
   }
   ```

   `pages/xxx/xxx.wxss`

   ```css
   .test {
     font-size: 38rpx;
     font-weight: 400;
     font-family: 'Montserrat';
   }
   ```

4. 效果

   ![](https://images.pandaomeng.com/279e75c751a7a3ae8d19ef13ab3555ea.jpg)

大功告成！