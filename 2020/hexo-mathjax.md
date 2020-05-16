---
title: hexo 中插入数学公式
date: 2020-05-16
tag: [笔记]
---

# hexo 中插入数学公式

## 前期准备

使用 hexo-renderer-pandoc 渲染的前提条件是，当前的操作系统安装过 pandoc

安装教程：https://github.com/jgm/pandoc/blob/master/INSTALL.md#linux

以 Linux 举例，从 [release](https://github.com/jgm/pandoc/releases/tag/2.9.2.1) 下载最新的包并安装

```
wget https://github.com/jgm/pandoc/releases/download/2.9.2.1/pandoc-2.9.2.1-1-amd64.deb
sudo dpkg -i pandoc-2.9.2.1-1-amd64.deb
```

## 配置

1. 替换 hexo 的渲染引擎，在 hexo 的项目根目录执行以下命令

   ```
   npm uninstall hexo-renderer-marked --save
   npm install hexo-renderer-pandoc --save
   ```

2. 在主题文件夹的目录下，编辑 _config 文件，打开 MathJax 的开关

   ```
   # MathJax Support
   mathjax:
     enable: true
     per_page: true
     cdn: //cdn.bootcss.com/mathjax/2.7.1/latest.js?config=TeX-AMS-MML_HTMLorMML
   ```

3. 在写博客的时候，在文章的 Front-matter 处声明开启 MathJax

   ```
   title: 切披萨的方案数题解
   date: 2020-05-14
   tag: [leetcode, dp]
   mathjax: true
   ```

4. 配置完成后执行重新生成，之前写好的公式就能正常显示了

   ```
   hexo clean
   hexo generate
   ```

## 相关工具

### [typora](https://typora.io/)

Markdown 编辑器，公式会被实时转义。效果如下图

![](https://images.pandaomeng.com/20200516151241.png)

