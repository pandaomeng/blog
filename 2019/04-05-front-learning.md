---
title: 前端面试题总结
date: 2019-04-05
tag: [web]
---

# 前端面试题总结

## JS基础知识点及常考面试题（一）

### 原始类型（Primitive）

1. 原始类型共6种：

   - undefined

   - null

   - boolean

   - string

   - number

   - symbol

2. 原始类型没有函数可以调用，但是在必要的情况下会进行强转为对象类型。

3. number为浮点类型，0.1 + 0.2 !== 0.3

4. string 类型是不可变的，无论在string类型上调用何种方法，都不会对值有改变。

5. null 不是对象，（typeof null === 'object' 是个历史遗留bug）

   >  JS 的最初版本中使用的是 32 位系统，为了性能考虑使用低位存储变量的类型信息，`000` 开头代表是对象，然而 `null` 表示为全零，所以将它错误的判断为 `object` 。虽然现在的内部类型判断代码已经改变了，但是对于这个 Bug 却是一直流传下来。

### 对象类型

1. 在JS中，除了原始类型，其他的都是对象类型。
2. 原始类型存储的是值，而对象类型存储的是地址（指针）
3. 函数传参是传递对象指针的**副本**

### typeof vs instanceof

1. typeof 对于原始类型来说，除了null都可以显示正确的类型

2. typeof 对于对象来说，除了函数都显示object，所以typeof并不能准确判断变量到底是什么类型

3. instanceof 可以判断一个对象的正确类型，但是无法判断原始类型（原理是原型链

4. instanceof 虽然无法直接判断原始类型，但是可以自己实现

   ```
   class PrimitiveString {
     static [Symbol.hasInstance](x) {
       return typeof x === 'string'
     }
   }
   console.log('hello world' instanceof PrimitiveString) // true
   ```

### 类型转换

1. JS 中类型转换只有三种情况，分别是：
   - 转换为布尔值
   - 转换为数字
   - 转换为字符串
2. 详情见下图类型转换表：

![](https://images.pandaomeng.com/b9beaa58e9b2cee998da395da7d7cb6c.jpg)

3. 对象类型的转换，会调用内置的[ToPrimitive]函数，对于该函数来说，算法逻辑一般来说如下：

   - 如果已经是原始类型了，那就不需要转换了
   - 调用 `x.valueOf()`，如果转换为基础类型，就返回转换的值
   - 调用 `x.toString()`，如果转换为基础类型，就返回转换的值
   - 如果都没有返回原始类型，就会报错

   当然我们也可以重写Symbol.toPrimitive，该方法在转原始类型时调用优先级最高。

   (待学习：toPrimitive 的 PreferredType 参数)

4. 四则运算时，有字符串则都为字符串，没字符串，就转为数字或字符串。
5. 字符串比较通过unicode字符索引进行比较

### this

![](https://images.pandaomeng.com/0690b06d50f58e11133374cad76c1072.jpg)

1. bind:

   > MDN的解释是：bind()方法会创建一个新函数，称为绑定函数，当调用这个绑定函数时，绑定函数会以创建它时传入 bind()方法的第一个参数作为 this，传入 bind() 方法的第二个以及以后的参数加上绑定函数运行时本身的参数按照顺序作为原函数的参数来调用原函数。





## JS基础知识点及常考面试题（二）

### == vs ===







## ES6知识点：

### var、let、const的区别

1. var会发生提升（hoisting），提升的是声明，函数也会被提升，并且优先于变量提升。
2. 函数的提升会把整个函数挪到作用域顶部，而变量提升只会把声明挪到作用域顶部。
3. 提升的存在是为了解决函数相互调用的情况。

4. let、const因为暂时性死区（ *temporal dead zone，简称* **TDZ**）的原因，不能在声明前使用。

   以下是es6对let/const声明中的解释：

   > The variables are created when their containing Lexical Environment is instantiated but may not be accessed inany way until the variable’s LexicalBinding is evaluated.

### 原型继承和Class继承

1. JS中不存在类，Class只是语法糖，本质还是函数
2. 