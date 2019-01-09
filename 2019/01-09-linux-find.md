---
title: linux find 命令记录
date: 2019-01-09
tag: [blog, hexo]
---

## 一、搜索文件夹内所有文件包含的一个字符串

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

## 二、搜索文件夹内某后缀名的文件，排除某个文件夹

```shell
find . -path ./node_modules -prune -o -name '*.json' -print
```



