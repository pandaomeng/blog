---
title: es6 Iterator generator
date: 2019-05-18
tag: [es6]
---

# es6 Iterator 和 Generator

## Iterator 基本概念

1. 遍历器（Iterator）就是一种统一的接口机制，任何数据结构只要部署 Iterator 接口，就可以完成遍历操作。
2. Iterator 的作用有三个：一是为各种数据结构，提供一个统一的、简便的访问接口；二是使得数据结构的成员能够按某种次序排列；三是 ES6 创造了一种新的遍历命令`for...of`循环，Iterator 接口主要供`for...of`消费。
3. 遍历器本质上是一个指针对象
4. 每一次调用next()方法，都会返回一个具有value和done属性的对象



## 基本概念

1. generator 函数和 普通函数写法不同， 在 function 关键字和 函数名之间有个 * 号
2. generator 函数的执行结果会返回一个遍历器对象，只有调用遍历器对象的 next() 方法
3. yield 只能用在 generator 函数里使用。