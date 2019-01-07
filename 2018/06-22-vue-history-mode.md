---
title: vue配置history模式
date: 2018-06-22
tag: [vue, vue-router]
---

## 前言

引用官方的解释：

[HTML5 History 模式](https://router.vuejs.org/zh/guide/essentials/history-mode.html)

`vue-router` 默认 hash 模式 —— 使用 URL 的 hash 来模拟一个完整的 URL，于是当 URL 改变时，页面不会重新加载。

默认的hash模式的url像这样：`http://yoursite.com/index.html#/user/id`

改为history模式后的url: `http://yoursite.com/user/id`

去掉#号的url好看了很多，那就让我们开始吧

<!--more-->

## 前端配置

### vue 路由配置修改

```
const router = new VueRouter({
  mode: 'history',
  routes: [...]
})
```

 ## 后端配置

### Apache 配置

1. 确保apache的rewrite模块开启

2. 在项目的更目录，和index.html同级，创建一个文件.htaccess

   ```
   <IfModule mod_rewrite.c>
     RewriteEngine On
     RewriteBase /
     RewriteRule ^index\.html$ - [L]
     RewriteCond %{REQUEST_FILENAME} !-f
     RewriteCond %{REQUEST_FILENAME} !-d
     RewriteRule . /index.html [L]
   </IfModule>
   ```

3. 重启apache2:

   ```
   service apache2 restart
   ```


## Nginx 配置：

1. 修改配置文件 `xxx.server`

   ```
   location / {
   	......
   	try_files $uri $uri/ /index.html;
   	root /home/sourcecode/dist;
   	......
   }
   ```

   其中 root路径 为项目打包好的文件夹路径