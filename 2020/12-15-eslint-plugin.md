---
title: eslint plugin 开发笔记
date: 2020-12-15
tag: [eslint]
---

# eslint plugin 开发笔记

## 新建项目

```
npm install -g yo generator-eslint
mkdir eslint-plugin-demo
cd eslint-plugin-demo
yo eslint:plugin
```

![](https://images.pandaomeng.com/create-eslint-plugin-20201216194949.png)

目录结构如下:

```
.
├── README.md
├── lib
│   ├── index.js
│   └── rules
├── package.json
└── tests
    └── lib
        └── rules
```

接着执行

```
yo eslint:rule
```

![](https://images.pandaomeng.com/yo-eslint-rule-20201216204945.png)

此时目录结构如下：

```
.
├── README.md
├── docs
│   └── rules
│       └── no-console-time.md
├── lib
│   ├── index.js
│   └── rules
│       └── no-console-time.js
├── package.json
└── tests
    └── lib
        └── rules
            └── no-console-time.js
```

## 无须发包，使用 npm link 在项目中别的项目做测试

在 eslint-plugin-demo 目录下执行

```
npm link
```

控制台会输出

```
/usr/local/lib/node_modules/eslint-plugin-demo -> /Users/your-name/workspace/eslint/eslint-plugin-demo
```

然后在需要使用该 npm 包的项目中执行

```
npm link eslint-plugin-demo
```

控制台会输出

```
/Users/your-name/workspace/eslint/eslint-example/node_modules/eslint-plugin-demo -> /usr/local/lib/node_modules/eslint-plugin-demo -> /Users/daomeng.pan/workspace/eslint/eslint-plugin-demo
```

如果需要解除 link ，则直接删除项目中 node_modules 下的软链接即可

```
rm /Users/your-name/workspace/eslint/eslint-example/node_modules/eslint-plugin-demo
```

## 编写 eslint rule

yo eslint:rule 生成的模板文件如下

```js
/**
 * @fileoverview this is a description
 * @author pandaomeng
 */
'use strict';

module.exports = {
  meta: {
    docs: {
      description: 'this is a description',
      category: 'Fill me in',
      recommended: false,
    },
    fixable: null, // or "code" or "whitespace"
    schema: [
      // fill in your schema
    ],
  },

  create: function (context) {
    return {
      // give me methods
    };
  },
};
```

- meta

  - docs
    - description: rule 的描述
    - category: rule 所属的类目，参考 eslint 官方提供的 rule，有：'Best Practices'、'Stylistic Issues'、'Possible Errors'、 'Variables'  、'Strict Mode'、'ECMAScript 6' 、'Deprecated' 、 'Removed'等，详见 [https://eslint.org/docs/rules/](https://eslint.org/docs/rules/)
    - recommended: 当 eslint 配置中开启 "extends": "eslint:recommended" 时，是否启用这条规则。
  - schema: 使用 [Json schema](http://json-schema.org/) 语法描述 eslint 可接收的 options 的结构
  - fixable: 在使用 `--fix` 的时候是否自动修复

- create: (function) 返回一个对象，其中包含了 ESLint 在遍历 JavaScript 代码的抽象语法树 AST ([ESTree](https://github.com/estree/estree) 定义的 AST) 时，用来访问节点的方法。

  - 如果一个 key 是个节点类型或 [selector](https://cn.eslint.org/docs/developer-guide/selectors)，在 **向下** 遍历树时，ESLint 调用 **visitor** 函数

  - 如果一个 key 是个节点类型或 [selector](https://cn.eslint.org/docs/developer-guide/selectors)，并带有 `:exit`，在 **向上** 遍历树时，ESLint 调用 **visitor** 函数

  - 如果一个 key 是个事件名字，ESLint 为[代码路径分析](https://cn.eslint.org/docs/developer-guide/code-path-analysis)调用 **handler** 函数

## create 方法举例

我们可以借助 [ast语法树分析网站](https://astexplorer.net/)，在里面得到我们的语法树

```js
module.exports = {
	// 省略...
  create: function (context) {
    return {
      Identifier: (node) => {
        const identifier = node.name
        // 可以对 identifier 做一些操作和判断，例如：
        if (identifier === 'foo') {
        	context.report({
            node: node.parent,
            message: '这是一个自定义报错: 不可以使用 {{name}} 作为标注符',
            data: {
            	name: identifier
            }
          });
        }
      },
    };
  },
};
```

## 对 import 语句的校验

使用 eslint-module-utils 提供的方法可以提高开发效率

例

```js
const { default: moduleVisitor } = require('eslint-module-utils/moduleVisitor');

module.exports = {
	// 省略...
  create(context) {
    const checkSourceValue = (source, importer) => {
      // 具体的校验逻辑, source 为引入的路径，importer 为整个 import 语法节点
    };
    return {
      ...moduleVisitor(checkSourceValue, context.options[0]),
    }
  },
};

```

参考源码 [https://github.com/benmosher/eslint-plugin-import/blob/master/utils/moduleVisitor.js](https://github.com/benmosher/eslint-plugin-import/blob/master/utils/moduleVisitor.js)

## context 对象

create 函数接收一个 context 对象

提供了很多常用的方法和属性，包括  getSourceCode() 获取源码、getFileName() 获取文件名、context.options 获取配置的参数(schema 中约定好的结构) 等

