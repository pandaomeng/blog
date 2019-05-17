---
title: es6 之 Symbol
date: 2019-05-17
tag: [es6]
---

# es6 之 Symbol

1. Symbol 是 es6 引入的新的一种原始数据类型

2. 他是一个独一无二的值

3. 不能使用 new Symbol()

4. Symbol() 可以接收一个字符串参数，但是只作为描述

5. Symbol不能和其他值做运算，但是可以显式地转为字符串

6. 也能转为boolean类型，为true

7. Symbol.prototype.description

   ```
   const sym = Symbol('foo')
   sym.description // 'foo'
   ```

8. 作为属性名的symbol必须加方括号，和字符串属性名区分

9. Symbol 作为属性名时不会被 for…in for…of 等 遍历到，但是可以被Object.getOwnPropertySymbols 取到，返回一个数组。

10. 奇怪的是，虽然不会被遍历到，但是symbol作为属性名是公有的。

11. Object.getOwnPropertyNames 可以获得enumerable 和 inenumerable的属性，但是拿不到Symble属性

12. Symbol.for() 可以重复利用Symbol

13. Symbol.keyFor(s1: Symbol): string | undefined 返回已登记的 Symbol 类型值的`key`

14. Symbol 用于单例模式，将Symbol.for('foo')作为global的一个字段，可以防止被误篡改

15. ES6 有11个内置的Symbol值

    - Symbol.hasInstance
    - Symbol.isConcatSpreadable，当定义在类上时，定义在实例和类上效果一样
    - Symbol.species
    - Symbol.match
    - Symbol.replace
    - Symbol.search
    - Symbol.split
    - Symbol.iterator
    - Symbol.toPrimitive
    - Symbol.toStringTag
    - Symbol.unscopables





