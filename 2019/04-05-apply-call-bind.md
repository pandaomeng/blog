---
title: js 的 apply, call 和 bind
date: 2019-04-05
tag: [js]
---

## js 的 apply, call 和 bind

1. call

   > **call()** 方法调用一个函数, 其具有一个指定的`this`值和分别地提供的参数(**参数的列表**)。

2. apply

   > **apply()** 方法调用一个具有给定`this`值的函数，以及作为一个数组（或[类似数组对象](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Indexed_collections#Working_with_array-like_objects)）提供的参数

3. bind

   > **bind()**方法创建一个新的函数，在调用时设置`this`关键字为提供的值。并在调用新函数时，将给定参数列表作为原函数的参数序列的前若干项。



一些有趣的栗子

1. 给定一个数组，通过 Math.max 找到最大值

   ```
   const numbers = [5, 458 , 120 , -215 ] 
   Math.max.apply(Math, numbers)   //458
   Math.max.call(Math, 5, 458 , 120 , -215) //458
   ```

   当然这里可以通过es6的结构语法

   ```
   Math.max(...numbers)
   ```

2. 数组之间追加

   ```
   const array1 = [12 , "foo" , {name: "Joe"} , -2458]
   const array2 = ["Doe" , 555 , 100]
   Array.prototype.push.apply(array1, array2)
   ```




总结：

- apply 、 call 、bind 三者都是用来改变函数的this对象的指向的；
- apply 、 call 、bind 三者第一个参数都是this要指向的对象，也就是想指定的上下文；
- apply 、 call 、bind 三者都可以利用后续参数传参；
- bind 是返回对应函数，便于稍后调用；apply 、call 则是立即调用 。