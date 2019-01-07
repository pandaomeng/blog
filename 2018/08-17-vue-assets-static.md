---
title: vuejs-templates静态资源目录src/assets、和static/区别
date: 2018-08-17
tag: [vue, webpack]
---

## src/assets/和static/区别和用法



### **一句话总结：第三方资源都放在static文件夹中（如脚本库），自己在项目中使用的一些资源都放在assets中**



### 文档传送门：

vuejs-templates官方英文文档：http://vuejs-templates.github.io/webpack/static.html

上面文档的中文翻译：https://athena0304.gitbooks.io/vue-template-webpack-cn/content/static.html

PS: 理论看上面的文档，这里就不复制粘贴了

<!--more-->

### 总结：

区别1：

​	通过assets引入的资源会被webpack打包（并且默认如果图片大小小于100000byte，会转为base64）

​		参考：	url-loader:  https://github.com/webpack-contrib/url-loader

​	通过static引入的资源会原封不动地在dist输出

区别2：

​	引用方式的不同，具体的引用方式如下：



### 用代码举个栗子：(用各种方式引图片）

#### 文件目录：

```
+-- src

|   +-- assets

|       +-- logo.png

|       +-- big_image.png

|   +-- HelloWorld.vue

+-- static

|   +-- images

|       +-- logo.png

```

#### 代码：

```
HelloWorld.vue
```

```
<template>
  <div>
    <img width="80px;" :src="logo1">
    <img width="80px;" :src="logo2">
    <img width="80px;" :src="logo3">
    <img width="80px;" :src="logo4">
    <img width="80px;" :src="logo5">
    <img width="80px;" src="../assets/logo.png">
    <img width="80px;" src="assets/logo.png">
    <img width="80px;" src="~assets/logo.png">
    <img width="80px;" src="/static/images/logo.png">
    <img width="80px;" :src="big_image">
  </div>
</template>

<script>
import logo3 from 'assets/logo.png'
export default {
  name: 'HelloWorld',
  data () {
    return {
      logo1: require('assets/logo.png'),
      logo2: require('../assets/logo.png'),
      logo3: logo3,
      logo4: '../assets/logo.png',
      logo5: '/static/images/logo.png',
      big_image: require('assets/big_image.png')
    }
  },
  created () {
    console.log('logo1: ', this.logo1)
    console.log('logo2: ', this.logo2)
    console.log('logo3: ', this.logo3)
    console.log('logo4: ', this.logo4)
    console.log('logo5: ', this.logo5)
    console.log('big_image: ', this.big_image)
  }
}
</script>

```

#### log输出如下：

![](http://images.pandaomeng.com/89ca7e16bd4742a1c2ae54330aa57b84.jpg)

(base64太长，就用图的形式贴出来了)

#### 页面显示如下：

![](https://images.pandaomeng.com/dda29bf7950fde866490a4b41cfe6ffc.jpg)

#### 分析：

- 分析 logo1, logo2, logo3, logo4 发现asssets只能通过require或者import引入，赋值字符串的方式行不通

- 使用如下这种方式可以不用require

  src中直接使用相对路径字符串，而不是将相对路径赋值给变量后再赋给src

  对比logo4 和 第六个logo，唯一的区别是后者没有使用变量

  ```
  <img width="80px;" src="../assets/logo.png">
  ```

  或者

  ```
  <img width="80px;" src="~assets/logo.png"> // 有符号 ~ 的加持
  ```

  PS: 必须在配置文件 `webpack.base.conf.js` 中设置别名

  ```
  resolve: {
      extensions: ['.js', '.vue', '.json'],
      alias: {
        'vue$': 'vue/dist/vue.esm.js',
        '@': resolve('src'),
        'assets': resolve('src/assets') // 这行划重点
      }
    },
  ```

- 分析logo1和big_image（大于10K）的控制台输出，logo1被转为base64，而big_image在构建的时候被"内联/复制/重命名"了。

- 错误的引用方式，

  上面第四种

  ```
  <img width="80px;" :src="logo4"> // logo4 为字符串变量
  ```

  上面第七种

  ```
  <img width="80px;" src="assets/logo.png"> // src同样为字符串，并且没有符号 ~ 的加持，路径不对
  ```
