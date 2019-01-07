---
title: vim 实用命令记录
date: 2018-11-26
tag: [vim]
---

## vim 的三种模式

Vim和Vi一样具有三种模式：命令模式（Command mode），插入模式（Insert mode）和底线命令模式（Last line mode）。

当用户处于不同模式的时候，敲击键盘会产生不同的作用。

详情查看[维基百科](https://zh.wikibooks.org/zh-hans/Vim/%E4%B8%89%E7%A7%8D%E6%A8%A1%E5%BC%8F)

<!--more-->



## 常用命令

## 一、全局替换（底线命令模式）：

```shell
:[addr]s/源字符串/目的字符串/[option]
```

> **[addr] 表示检索范围，省略时表示当前行。**
>
> 如：“1，20” ：表示从第1行到20行；
>
> “%” ：表示整个文件，同“1,$”；
>
> “. ,$” ：从当前行到文件尾；
>
> **s : 表示替换操作**
>
> **[option] : 表示操作类型**
>
> 如：g 表示全局替换; 
>
> 省略option时仅对每行第一个匹配串进行替换；
>
> 如果在源字符串和目的字符串中出现特殊字符，需要用”\”转义

举个栗子：

全局替换 apple 为 banana

```shell
:%s/apple/banana/g
```

## 二、搜索文件夹内所有文件包含的一个字符串

目录下的所有文件中查找字符串

```shell
find . | xargs grep -r "class" 
```

目录下的所有文件中查找字符串,并且只打印出含有该字符串的文件名

```shell
find . | xargs grep -r "class" -l 
```

> xargs 将 `find .` 的结果拼成一行，以空格为分隔，拆成多个参数，传给grep（注：如果文件名中有空格，会出bug。
>
> grep -r （同rgrep 弃用）打印出匹配正则的行