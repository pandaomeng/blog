---
title: js prototype
date: 2019-05-18
tag: [js]
---

# JS 的原型

## 对象原型

1. 对象的原型可以通过 Object.getPrototypeOf(obj) 或者 obj.\_\_proto\_\_（弃用）得到

2. 区别对象的原型 obj.\_\_proto\_\_ 和构造函数的 prototype 属性

   > Object.getPrototypeOf(new Foobar()) 与 Foobar.prototype 指向同一个对象

3. 对象原型 即 对象的原型 即 对象的原型对象，因为原型也是一个对象

4. 区别 doSomething() 和 new doSomething()

5. 原型对象：构造函数的prototype属性所指向的对象

6. Object.create() 从指定的原型对象创建一个新的对象

   ```
   var car = Object.create(Car.prototype)
   ```

7. constructor 是构造函数自带的属性，可以通过 constructor.name 获取构造器名字

8. js 通过原型链向上查找属性

9. 原型的 `constructor` 属性指向构造函数，构造函数又通过 `prototype` 属性指回原型

## new 操作符

1. 用法：

   > new constructor[([arguments])]

2. new 原理

   - 创建一个空的对象(即 {} )
   - 将对象的 \_\_proto\_\_ 指向构造函数的prototype，即继承
   - 带参数调用构造函数的constructor（即构造函数本身），但是this的值指向刚创建的那个对象
   - 如果constructor有返回，则 new 的结果就用他的返回值，如果没有就使用this

   



