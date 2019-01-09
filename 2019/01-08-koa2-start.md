---
title: koa2 初尝试
date: 2019-01-08
tag: [node, koa2]
---

# koa2 初尝试

文档：https://chenshenhai.github.io/koa2-note/

Koa2-generator 项目：https://github.com/17koa/koa-generator

## 一、初始化项目

全局安装koa-generator:

```shell
npm install -g koa-generator
```

初始化项目：

```shell
koa2 your_project/
```

<!--more-->



## 二、配置eslint

1. 全局安装eslint

   ```shell
   npm install -g eslint
   ```

2. 进入项目根目录，执行以下命令：

   ```
   eslint --init
   ```

   根据提示选择你需要的配置，例如：

![](https://images.pandaomeng.com/ff46e6c99b7d52a43534d2eef3b03098.jpg)

​	我这里选择airbnb作为我的代码规范。

​	至此，项目里已经生成了.eslintrc.js.

3. 在该文件中做一些自定义的修改

   ```
   module.exports = {
     "extends": "airbnb-base",
     // add your custom rules here
     rules: {
       "no-console": "off",
       "semi": ["error", "never"],
     }
   };
   ```

4. 在package.json中配置lint和自动fix命令（scripts下）：

   ```
   "lint": "eslint --ext .js --ignore-path .gitignore .",
   "fix": "eslint --ext .js --ignore-path .gitignore . --fix"
   ```

   接下来在项目根目录执行

   ```
   npm run fix
   ```

   根据提示修改代码使他符合规范

5. 在项目根目录添加.eslintignore，使个别文件不加入lint，文件内容：

   ```
   /bin/www
   ```


## 三、启动项目

1. 在项目根目录执行：

   ```
   npm run start
   ```

![](https://images.pandaomeng.com/8974d8b6097dab225aac12648a21d309.jpg)

​	即启动完毕了。

2. 通过端口号3000进行访问查看

![](https://images.pandaomeng.com/cee5ad3359b09744da76c1b7df95e939.jpg)

