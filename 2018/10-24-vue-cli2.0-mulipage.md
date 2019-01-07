---
title: vue-cli 2.0 配置多页应用
date: 2018-10-24
tag: [vue, vue-cli]
---

# vue-cli 2.0 配置多页应用

## 前言

用 vue-cli 2.0 搭建了一个后台管理系统，（之前搭的项目，当时vue-cli 3.0还没出）

`App.vue`大概是这样的结构

```html
<template>
  <div>
    <nav-bar></nav-bar>
    <div class="content">
      <router-view/>
    </div>
  </div>
</template>
```

侧边栏 NavBar 常驻，content部分通过vue-router动态改变。但是这样就出现了个问题，登录页不想要NavBar该怎么办呢？我们配置多页，将login页面独立于原本的单页应用。

<!--more-->

## 配置之前先创建文件

`login.html` 、 `src/login.js`、`src/Login.vue`

`login.html`

```html
<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <title>Meizhua小程序管理后台</title>
</head>

<body>
  <div id="login">
    <login></login>
  </div>
</body>

</html>

```

`src/login.js`

```js
import Vue from 'vue'
import Login from './Login.vue'

/* eslint-disable no-new */
new Vue({
  el: '#login',
  components: { Login },
  template: '<login/>'
})
```

`src/Login.vue`

```js
<template>
  <h1>
    Login Page
  </h1>
</template>
```

## 配置webpack

1. `build/webpack.base.conf.js` 的entry中添加新入口

```js
entry: {
  app: './src/main.js',
  login: './src/login.js'
},
```

2. `build/webpack.dev.conf.js` 的plugin中添加新的HtmlWebpackPlugin

```js
plugins: [
  // 其他配置省略
  new HtmlWebpackPlugin({
    filename: 'index.html',
    template: 'index.html',
    inject: true,
    chunks: ['app']
  }),
  new HtmlWebpackPlugin({
    filename: 'login.html',
    template: 'login.html',
    inject: true,
    chunks: ['login']
  })
]
```

注：chunks数组表示每个单页都只加载自己的模块

3. `build/webpack.prod.conf.js` 同理配置

```
plugins: [
  // 其他配置省略
  new HtmlWebpackPlugin({
    filename: config.build.index,
    template: 'index.html',
    inject: true,
    minify: {
      removeComments: true,
      collapseWhitespace: true,
      removeAttributeQuotes: true
    },
    chunks: ['manifest', 'vendor', 'app'],
    chunksSortMode: 'dependency'
  }),
  new HtmlWebpackPlugin({
    filename: config.build.login,
    template: 'login.html',
    inject: true,
    minify: {
      removeComments: true,
      collapseWhitespace: true,
      removeAttributeQuotes: true
    },
    chunks: ['manifest', 'vendor', 'login'],
    chunksSortMode: 'dependency'
  })
]
```

4. `config/index.js` 的build中配置

```js
  build: {
    // Template for index.html
    index: path.resolve(__dirname, '../dist/index.html'),
    login: path.resolve(__dirname, '../dist/login.html'),
    // 其他配置省略 ......
  }
```

## 效果图

热加载的dev环境：

![多页配置](https://images.pandaomeng.com/944df5d572ef76869f847756b86f0233.jpg)

大功告成啦~



