---
title: 前端面试题整理
date: 2020-06-26
tag: [笔记]
---

# 前端面试题整理

## call、apply、bind

### 手动实现 call

```
Function.prototype.myCall = function (context, ...args) {
		context = context || window
    let symbol = Symbol()
    context[symbol] = this
    context[symbol](...args)
    delete context[symbol]
}

function sayHi () {
    console.log(`Hi, ${this.name}`)
}
var a = {name: 'Patrick'}
sayHi.myCall(a)
```

- Symbol() 处可以使用一个空对象代替，因为对象的引用地址一定是唯一的。

### 手动实现 apply

同 call 的实现，只有接收参数的地方不同。

```
Function.prototype.myApply = function (context, args) {
		......
}
```

### 手动实现 bind

```
var myBind = function (context, ...presetArgs) {
    return (...args) => {
        this.call(context, ...presetArgs, ...args)
    }
}

Function.prototype.myBind = myBind

function foo(arg1, arg2) {
    console.log(this, arg1, arg2)
}

// case 1
foo('hello', 88)

// case 2
var a = {}
foo.myBind(a, 'world')(99)
```

[使用 babel 转义为 es5 代码](https://babeljs.io/repl#?browsers=&build=&builtIns=false&spec=false&loose=false&code_lz=G4QwTgBAtgngQgSwHYBMIF4IDMCuSDGALggPZIQAU-ZhApgB6EA0EAdOwA5i0DOthAQTABzHgEoIAbwBQEORG6EcYchXatwoiegB8U2fMOEAFgh6t8IADZWqNBszadufQSJ4t1m8QbkBfaQDpADE8IlIkVi4SQhiYDlpWWERUDGh4ZBRpIA&debug=false&forceAllTransforms=false&shippedProposals=false&circleciRepo=&evaluate=false&fileSize=false&timeTravel=false&sourceType=module&lineWrap=true&presets=es2015&prettier=false&targets=&version=7.10.3&externalPlugins=)

* 注意第二行返回的是一个箭头函数，这边的 this 才是 foo，否则的话 this 会指向 window。
*  bind 可以传预设的参数列表，可以用作函数 curly 化。
* 连续的 bind 只有第一个 bind 会生效，解释如下：
  * es6 的代码中，使用的是箭头函数，this 指向在调用第一个 bind 的时候就确定了。
  * es5 的代码中，函数调用的时候，后面的 bind 返回的函数反而会先执行，最后实际起作用的是第一个 bind 绑定的对象。

#### TODO

[-] 如何实现连续 bind，this 指向最后一个绑定的对象？



##  => 箭头函数的实现



## 继承相关

1.  有几种继承方式
2. 