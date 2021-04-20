---
title: typescript
date: 2021-04-18
tag: [typescript]
---

# Typescript 面试知识点梳理

## type 和 interface 的区别

1. 描述的对象不同
   - interface 只能描述对象和函数
   - type 可以描述所有的类型
2. 特性不同
   - interface 声明相同的类型名的时候可以合并，如果有属性类型冲突会报错
   - type 相比于 interface，可以声明 别名、联合类型和元组，以及可以使用 typeof 获取变量的类型
3. 对于扩展的写法不同
   - interface 使用 extends 进行扩展
   - type 使用 &（交叉类型） 进行扩展





