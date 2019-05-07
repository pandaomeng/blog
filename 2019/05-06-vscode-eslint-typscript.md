---
title: VScode下搭配ESLint、typescript-eslint的代码检查配方
date: 2019-05-06
tag: [ts]
---

# VScode下搭配ESLint、typescript-eslint的代码检查配方

使用eslint监测ts文件

## typescript的全局使用

```
npm i -g typescript
```

通过tsc命令查看全局ts版本

```
tsc -v
```

![](https://images.pandaomeng.com/blog/tsc-v.png)

## 新建node项目

1. 建一个空的文件夹ts-eslint

2. 在文件夹下执行node初始化命令

   ```
   npm init
   ```

   一路回车后，node项目就初始化完毕了

3. 通过命令 tsc --init 初始化一个tsconfig.json文件

<!--more-->

## ESLint

1. 全局安装eslint

   ```
   npm i -g eslint
   ```

2. 在项目根目录执行eslint的初始化命令

   ```
   eslint --init
   ```

   这里我选择使用airbnb的规范作为我的代码规范

   ![](https://images.pandaomeng.com/blog/eslint--init2.png)

3. 等它安装完毕之后，自动生成了eslint的配置文件，如下：

   ![](https://images.pandaomeng.com/blog/after-eslint-init.png)

4. 我们给他添加一些配置（根据自己喜好来）

   这里我禁用了"no-console"这个规则

   ```
   module.exports = {
       "extends": "airbnb-base",
       rules: {
           "no-console": "off",
       }
   };
   ```

5. vscode安装eslint扩展

   ![](https://images.pandaomeng.com/blog/vscode-eslint-extension.png)

6. 打开vscode的配置文件setting.json

   通过快捷键（cmd + shift + P）输入settings，可以快速打开

   ![](https://images.pandaomeng.com/blog/vscode-setting.png)

   在配置文件中加入下列内容

   ```
   {
   	"eslint.autoFixOnSave": true,
   	"eslint.validate": [
           "javascript",
           "javascriptreact",
      	    {
               "language": "typescript",
               "autoFix": true
           }
       ]
   }
   ```

   到这里，eslint就配置成功了，你可以自己创建一个js文件进行测试


## Eslint的typescript插件

无需再装其他ts相关的插件了。

参考文档 [typescript-eslint如何使用](https://github.com/typescript-eslint/typescript-eslint/tree/master/packages/eslint-plugin#usage)

1.  确保已经安装TypeScript and @typescript-eslint/parser，然后安装@typescript-eslint/eslint-plugin

   ```
   npm i typescript --save-dev
   npm i @typescript-eslint/parser --save-dev
   npm i @typescript-eslint/eslint-plugin --save-dev
   ```

2. 修改.eslintrc配置文件

   ```
   module.exports = {
       "extends": ["airbnb-base", "plugin:@typescript-eslint/recommended"],
       "parser": "@typescript-eslint/parser",
       "plugins": ["@typescript-eslint"],
       rules: {
           "no-console": "off",
           "@typescript-eslint/indent": ["error", 2],
       }
   };
   ```

3. 配置完毕，项目中最终的依赖如下

   ```
   "devDependencies": {
       "@typescript-eslint/eslint-plugin": "^1.7.0",
       "@typescript-eslint/parser": "^1.7.0",
       "eslint": "^5.16.0",
       "eslint-config-airbnb-base": "^13.1.0",
       "eslint-plugin-import": "^2.17.2",
       "typescript": "^3.4.5"
     }
   ```



参考：[eslint的规则](https://eslint.org/docs/rules/) 、[typescript-eslint的规则](https://github.com/typescript-eslint/typescript-eslint/tree/v1.7.0/packages/eslint-plugin/docs/rules)、[airbnb的规则](https://github.com/airbnb/javascript)









