---
title: 临时解决gitalk插件 cdn 地址挂掉的问题
date: 2019-01-09
tag: [blog, hexo]
---

# 临时解决gitalk插件 cdn 地址挂掉的问题

1. 进入主题目录，执行下面的命令，找到对应的url配置的文件

   ```shell
   find . -path './node_modules' -prune -o -print | xargs grep -r 'https://unpkg.com/gitalk/dist/gitalk.css'
   ```

   ![](https://images.pandaomeng.com/fde56d5000220ecfc9e8a3e469bbe893.jpg)

   <!--more-->

2. 编辑该文件

   ```sbhell
   vim ./layout/_third-party/comments/gitalk.swig
   ```

   修改cdn地址：

   ```
   <link rel="stylesheet" href="https://unpkg.com/gitalk/dist/gitalk.css">
   # 替换为
   <link rel="stylesheet" href="https://images.pandaomeng.com/gitalk.css">
   ```

   ![](https://images.pandaomeng.com/6eda79795387d2b858487b62c5b0bbc6.jpg)

   我这里替换为我自己的cdn，这样就临时解决了。

