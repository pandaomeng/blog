---
title: webpack 知识点梳理
date: 2021-04-10
tag: [webpack]
---

# webpack 知识点梳理

## 基本配置

### 拆分配置和 merge

拆分配置为 webpack.common.js、webpack.dev.js、webpack.prod.js

webpack.dev.js、webpack.prod.js 通过  webpack-merge 的 smart 继承 webpack.common.js

```js
import { smart } from 'webpack-merge'
```

在 webpack5 中，更改为

```js
import merge from 'webpack-merge'
```

dev、prod 区别

mode、plugin: webpack.DefinePlugin、devServer

### 启动本地服务

安装 webpack-dev-server，通过 --config 指定 config 文件

proxy 端口代理

### 处理 es6

对 js 文件处理，使用 babel-loader

使用 babelrc 文件进行配置 preset & plugin

### 处理样式

css:

Loader: ['style-loader', 'css-loader', 'postcss-loader']

postcss-loader: 配置文件 postcss.config.js 其内容  plugins: [require('autoprefix')]

less:

Loader: ['style-loader', 'css-loader', 'less-loader']

loader 执行顺序从后往前

### 处理图片

url-loader: 小于指定 limit 大小的图片使用 base64，可以通过指定 output 把文件统一放在某个目录下

url-loader file-loader 区别

它们都可以用于载入静态资源文件，url-loader 比 file-loader 的优势在于可以将文件转为 base64

they are both deprecated in webpack v5，使用 asset module 代替

url-loader file-loader 原理：

loader 本质上是一个函数，它们遇到需要解析的文件，会使用 webpack loader 的 api emitFile 将文件移动到指定的路径，并将文件改写为以下内容

```js
export default '/absolute/path'
```

or

```js
export default 'data:imgae/jpeg:base64....'
```

这样的内容。

### output

```js
output: {
  filename: 'bundle.[currentHash:8].js'
}
```

使用 currentHash 的好处：当内容不变时，hash 值不变，浏览器可以使用缓存。

### 模块化

TODO: 为什么可以使用 import output 的语法？



## 高级配置

### 多入口

1. 需要配置多个 entry

   ```js
   // webpack.common.js
   entry: {
     index: path.join(srcPath, 'index.js'),
     other: path.join(srcPath, 'other.js'),
   }
   ```

2. 对应的 output 需要需改

   ```js
   // webpack.prod.js
   output: {
     filename: 'bundle.[currentHash:8].js',
     path: distPath, // 输出文件路径
   }
   ```

   使用 currentHash 的好处：当内容不变时，hash 值不变，浏览器可以使用缓存。

3. HtmlWebpackPlugin 需要多个

   ```js
   // webpack.common.js
   // 配置多入口
   plugins: [
     new HtmlWebpackPlugin({
       template: path.join(srcPath, 'index.html'),
       filename: 'index.html',
       // 如果不指定 chunks，会引入所有 chunks
       chunks: ['index']
     }),
     new HtmlWebpackPlugin({
       template: path.join(srcPath, 'other.html'),
       filename: 'other.html',
       chunks: ['other']
     })
   ]
   ```

4. 其他

   CleanWebpackPlugin: 会清空 output.path 指定的路径 

### 抽离压缩 css 文件

使用 MiniCssExtractPlugin

```js
rules: [
  {
    test: '/\.less$/',
    loader: [
      MiniCssExtractPlugin.loader,
      'css-loader',
      'less-loader',
      'postcss-loader'
    ]
  }
],
plugin: [
  // 抽离 css 文件
  new MiniCssExtractPlugin: {
      filename: 'css/main.[contenthash:8].css'
  }
],
optimization: {
  minimizer: [new TerserJsPlugin({}), new OptimizeCssAssetPlugin()]
}
```

### 抽离公共代码

公用代码和第三方代码不会经常变，如果不抽离，每次都需要被重新打包，因为每次的hash值都发生变化，所以不能使用缓存，浏览量每次都会加载很慢。

```js
plugins: [
  new HtmlWebpackPlugin({
    template: path.join(srcPath, 'index.html'),
    filename: 'index.html',
    chunks: ['index', 'vender', 'common']
  }),
  new HtmlWebpackPlugin({
    template: path.join(srcPath, 'other.html'),
    filename: 'other.html',
    chunks: ['other', 'common']
  })
],
optimization: {
  splitChunks: {
    /** initial 入口 chunk，对于异步导入的 chunk 不处理
      * async 异步 chunk，只对异步导入的文件处理
      * all 全部 chunk
      */
    chunks: 'all',
    // 缓存分组
    cacheGroups: {
      vendor: {
        // 第三方模块
        vendor: {
          name: 'vender', // chunk 名称
          priotrity: 1, // 权限更高，优先抽离，重要！！！
          test: /node_modules/, // 
          minSize: 10 * 1024, // 模块的大小限制
          minChunks: 1 // 最少复用过多少次
        },
        // 公用模块
        common: {
          name: 'common', // chunk 名称
          priotrity: 0, // 优先级
          test: /node_modules/, // 
          minSize: 10 * 1024, // 模块的大小限制
          minChunks: 2 // 公共模块最少复用过多少次
        }
      }
    }
  }
}
```

### 怎么懒加载

使用方式:

```js
setTimeout(() => {
  // vue react 异步组件也是通过这种方式
  // 使用 webpackChunkName 指定 chunk name
  import(/* webpackChunkName: "dynamic" */ './dynamic-data.js').then(res => {
    console.log(res.default.message)
  })
}, 5000)
```

### 处理 jsx 和 vue

1. react 使用 @babel/preset-react

   ```
   // .babelrc
   {
     "presets": ["@babel/preset-react"],
     "plugins": []
   }
   ```

2. vue 使用 vue-loader

   ```js
   module: {
     rules: [
       {
         test: '/\.vue$/',
         loader: ['vue-loader'],
         include: srcPath
       }
     ]
   }
   ```



## 性能优化 - 优化打包效率

### 1. 优化 babel-loader

```js
{
  test: /\.js$/,
  loader: ['babel-loader?cacheDirectory'], // 开启缓存
  include: srcPath, // 明确范围
  // 或者
  exclude: /node_module/
}
```

- 开启缓存
- 限定范围

### 2. IgnorePlugin

避免引用无用模块

例子：

```js
// webpack.prod.js
plugins: [
  // 忽略 moment 的 locale 目录
  new webpack.IgnorePlugin(/\.\/local/, /moment/)
]
```

使用的地方

```js
import moment from 'moment' // 235.4K (gzipped: 66.3K)
import 'moment/locale/zh-cn'
moment.locale('zh-cn') // 设置语言为中文
```

### 3. noParse

```js
module: {
  // 对于 react.min.js 不打包
  noParse: [/react\.min\.js$/]
}
```

- IgnorePlugin 直接不引于，代码中没有（同时优化产出代码）
- noParse 引入，但不打包

### 4. HappyPack 多进程打包

JS 是单线程的，要开启多进程打包

提高构建速度，特别是对于多核 CPU

```js
// webpack.prod.js
const HappyPack = require('happypack')
```

```js
// webpack.prod.js
module: {
  rules: [
    {
      test: /\.js$/,
      use: ['happypack/loader?id=babel'],
      include: srcPath,
    }
  ]
},
plugins: [
  new HappyPack({
    // 用唯一的标标识符 id 来代表当前的 HappyPack 是用来处理哪一类特定的文件
    id: 'babel',
    // 如何处理 .js 文件，用法核 loader 配置一样
    loaders: ['babel-loader?cacheDirectory']
  })
]
```

### 5. ParallelUglifyPlugin 多进程压缩代码

使用多进程压缩代码

```js
// webpack.prod.js
const ParallelUglifyPlugin = require('ParallelUglifyPlugin')
```

```js
// webpack.prod.js
plugins: [
  new ParallelUglifyPlugin({
    // 本质上还是用了 UglifyJS，以下是传给 UglifyJS 的参数，优化的点在于开启了多进程
    uglifyJS: {
      output: {
        beautify: false, // 紧凑的输出
        comments: false, // 删除所有的注释
      },
      compress: {
        drop_console: true,
        // 内嵌定义了但是只用到一次的变量
        collapse_vars: true,
        // 提取出出现多次，但是没有定义成变量去引用的静态值
        reduce_vars: true,
      }
    }
  })
]
```

PS: 

- 项目较大，打包较慢，开启多进程能提高速度
- 项目较小，打包很开，开启多进程返回会降低速度（进程开销）
- 按需使用

### 6. webpack 配置自动刷新和热更新

自动刷新 vs. 热更新

- 自动刷新: 整个网页全部刷新，速度慢，状态丢失
- 热更新: 新代码生效，页面不刷新，速度快，状态不丢失

wepack 有自带的配置字段 watch 配置自动刷新

```js
module.exports = {
  entry: {
    // index: path.join(srcPath, 'index.js'),
    index: [
      'webpack-dev-server/client?http://localhost:8080/',
      'webpack/hot/dev-server',
      path.join(srcPath, 'index.js'),
    ],
    other: path.join(srcPath, 'other.js'),
  },
  devServer: {
    port: 8080,
    progress: true, // 显示打包的进度条
    contentBase: distPath, // 根目录
    open: true, // 自动打开浏览器
    compress: true, // 启动 gzip 压缩

    hot: true,

    // 设置代理
    proxy: {
      // 将本地 /api/xxx 代理到 localhost:3000/api/xxx
      '/api': 'http://localhost:3000',

      // 将本地 /api2/xxx 代理到 localhost:3000/xxx
      '/api2': {
        target: 'http://localhost:3000',
        pathRewrite: {
          '/api2': '',
        },
      },
    },
  },
};
```

使用的地方

```js
import { sum } from './math'

const sumRes = sum(10, 20)
console.log('sumRes', sumRes)

// 增加开启热更新之后的代码逻辑
if (module.hot) {
  module.hot.accept(['./math'], () => {
    const sumRes = sum(10, 30);
    console.log('sumRes in hot', sumRes);
  });
}
```

### 7. 使用 DllPlugin

动态链接库插件

- 前端框架如 vue、react 体积大、构建慢
- 比较稳定，不常升级版本
- 同一个版本只构建一次即可，不用每次都重新构建

webpack 已经内置 DllPlugin 支持

步骤：

1. 通过 DllPlugin 打包出 dll 文件

2. 通过 DllReferencePlugin 引用 dll 文件

## 性能优化 - 优化产出代码

好处：

- 体积更小
- 合理分包，不重复加载
- 速度更快、内存使用更少

### 1. 小图片 base64 编码

使用 url-loader 配置 limit

### 2. bundle 使用 hash

可以使用缓存

### 3. 懒加载

```js
import('./dynamic-data.js').then()
```

### 4. 提取公共代码

optimization.splitChunks

### 5. IgnorePlugin

IgnorePlugin 直接不引于，代码中没有无用的代码

### 6. 使用 cdn 加速

```js
output: {
  filename: '[name].[contentHash:8].js',
  path: distPath,
  publicPath: 'http://cdn.abc.com'  // 修改所有静态文件 url 的前缀（如 cdn 域名），这里暂时用不到
},
```

1. output 中配置 publicPath，以及静态资源配置 publicPath
2. 上传静态文件

### 7. 使用 mode production（包括 Tree-Shaking）

- 自动开启代码压缩
- Vue React 等会自动删掉调试代码（如开发环境的 warning）
- 启用 Tree-Shaking，只有 ES6 Module 才能让 tree-shaking 生效

Tree-Shaking 举例

```js
// math.js
export const add = (a, b) => a + b;

export const multi = (a, b) => a * b
```

在实际使用的地方只有使用到 add，那么 multi 就不会被打包

#### ES6 Module vs. Commonjs

- ES6 Module 静态引入，编译时引入
- commonjs 动态引入，执行时引入
- 只有 ES6 Module 才能静态分析，实现 Tree-shaking

### 8. Scope Hoisting

打包出来的代码中，每个文件一个函数会合并成一个函数

好处：

- 代码体积更小
- 创建函数作用域更少
- 代码可读性好

```js
const ModuleConcatenationPlugin = require('webpack/lib/optimize/ModuleConcatenationPlugin')

module。exports = {
  resolve: {
    // 针对 npm 中的第三方模块优先采用 jsnext:main 中指向的 ES6 模块化语法的文件
    mainFields: ['jsnext:main', 'browser', 'main']
  },
  plugins: [
    // 开启 scope hoisting
    new ModuleConcatenationPlugin()
  ]
}
```



## babel

babel 只对语法进行转换，对于新的 api 不进行转换，例如 Promise 或者 [].includes

新的语法需要使用 babel-polyfill 进行转换

babel 也不管模块化，所以需要配合 webpack 使用

### 环境搭建 & 基本配置

.babelrc

```json
{
  "presets": [
    ["@babel/preset-env"]
  ],
  "plugins": []
}
```

使用  babel 转换 js 文件

```shell
npx babel index.js
```

### babel-polyfill

polyfill 即布丁，babel-polyfill = core-js + regenerator，在 babel 7.4 之后弃用，推荐直接使用 core-js & regenerator

#### core-js & regenerator

core-js 是一系列 polyfill 的合集

regenerator 对 generator 语法进行补充支持

#### 配置按需引入

.babelrc

```json
{
  "presets": [
    [
      "@babel/preset-env", 
      {
        "useBuiltIns": 'usage',
        "corejs": 3,
      }
    ]
  ],
  "plugins": []
}
```

### babel-runtime

babel-polyfill 会污染全局环境，

使用 babel-runtime 不会去污染全局环境

如果开发第三方库，必须使用 babel-runtime



## 构建流程概述





## webpack 5

主要是内部效率优化



## 面试真题

### 1. 前端为何要进行打包和构建

1. 代码层面
   - 体积更小（tree-shaking、压缩、合并），加载更快
   - 编译高级语言和语法（TS、ES6、模块化、scss）
   - 兼容性和错误检查（polyfill、postcss、eslint）
2. 研发流程方面
   - 统一高效的开发环境
   - 统一的构建流程和产出标准
   - 集成公司的构建规范（提测、上线等）

### 2. module chunk bundle 的区别

- module 各个源码文件，webpack 中一切皆模块
- chunk 多模块合并成的，来源：entry、import()、splitChunk
- bundle 最终的输入文件，一般一个 chunk 对应一个 bundle

chunk 是偏抽象的一个概念，bundle 是 chunk 的输出实体

### 3. loader vs. plugin

loader: 模块转换器  如：less => css

plugin: 扩展插件，如 HtmlWebpackPlugin

### 4. 常见的 loader 和 plugin 有哪些

略

### 5. babel vs. webpack

- babel 只关心语法，不关心 api 不关心模块
- webpack 打包构建工具，是多个 loader plugin 的集合，通常会结合 babel 使用

### 6. 如何产出一个 lib

配置 output.library

### 7. babel-polyfill vs. babel-runtime

略

### 8. webpack 如何实现懒加载 & vue react 异步加载路由

import()

### 9. 为啥 proxy 不能被 polyfill

如 class 可以用 function 模拟

如 Promise 可以用 callback 模拟

但是 proxy 的功能无法用 Object.defineProperty 模拟

### 10. webpack 优化构建速度

略





 

