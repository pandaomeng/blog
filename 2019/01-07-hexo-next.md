---
title: 打造属于你的专属博客，Hexo，Next主题
date: 2019-01-07
tag: [blog, hexo, next]
---

# 打造属于你的专属博客，Hexo，NexT主题

## 一、安装NexT主题

1. 安装

   ```shell
   cd <your-hexo-site>
   git clone https://github.com/iissnan/hexo-theme-next themes/next
   ```

2. 检查版本

   ```shell
   cd themes/next/
   grep version _config.yml
   ```

<!--more-->

## 二、配置NexT

参考NexT的官方文档：

https://theme-next.iissnan.com/third-party-services.html

**注意：以下配置就要重新生成后生效（hexo g)**

### 1) 修改一些基础配置

1. 编辑 `themes/next/_config.yml`

   ```
   # Site
   title: 无趣的小帕
   subtitle:
   description: Just do it!
   keywords: IT
   author: pandaomeng
   language: zh-Hans
   timezone:
   ```

   ```git
   seo: true
   ```

   ```git
   menu:
     tags: /tags/ || tags
   ```

   ```git
   Schemes
     scheme: Gemini
   ```

   ```git
   motion:
     enable: false
   ```

### 2）接入百度统计

 1. 登录百度站长统计，点击新建站点

	2. 查看统计代码，获取 Baidu Analytics ID

    ![](https://images.pandaomeng.com/0b0bf4f2cde82ccfca87eaca239e448d.jpg)

3. 编辑 `themes/next/_config.yml`

   ![](https://images.pandaomeng.com/f6147aae2be0d46099fa12c8fdce8c50.jpg)

### 3）接入busuanzi访问统计

1. 编辑 `themes/next/_config.yml`

   ```
   busuanzi_count:
     # count values only if the other configs are false
     enable: true
     # custom uv span for the whole site
     site_uv: true
     site_uv_header: <i class="fa fa-user"></i> 访问人数
     site_uv_footer:
     # custom pv span for the whole site
     site_pv: true
     site_pv_header: <i class="fa fa-eye"></i> 总访问量
     site_pv_footer: 次
     # custom pv span for one page only
     page_pv: true
     page_pv_header: <i class="fa fa-file-o"></i> 浏览
     page_pv_footer: 次
   ```

2. busuanzi 的域名失效问题解决，见issue

   https://github.com/iissnan/hexo-theme-next/issues/2174

### 4）接入搜索功能

1. 在博客根目录下执行以下命令：

   ```
   npm install hexo-generator-searchdb --save
   ```

2. 编辑根目录的配置文件 `_config.yml`

   可以加在文件最后

   ```
   search:
     path: search.xml
     field: post
     format: html
     limit: 10000
   ```

3. 编辑主题配置文件 `themes/next/_config.yml`

   ```
   # Local search
   local_search:
     enable: true
   ```

4. 效果图

   ![](https://images.pandaomeng.com/4b8cfc17666b09d32d26ac8520f603aa.jpg)

### 5）接入上图的萌妹子看板娘

github仓库：https://github.com/EYHN/hexo-helper-live2d

model预览：https://huaji8.top/post/live2d-plugin-2.0/

1. 往根目录的配置文件中加入

   ```
   live2d:
     enable: true
     scriptFrom: local
     pluginRootPath: live2dw/
     pluginJsPath: lib/
     pluginModelPath: assets/
     tagMode: false
     log: false
     model:
       use: live2d-widget-model-shizuku
     display:
       position: right
       width: 150
       height: 300
     mobile:
       show: true
   ```

   注：上面用的model是live2d-widget-model-shizuku，所以我们要install她

2. 载入model

   在根目录：

   ```shell
   npm install --save hexo-helper-live2d
   npm install --save live2d-widget-model-shizuku
   ```

### 6）接入动态评论系统gitalk

​	篇幅较长，链接到另一篇文章：

​	https://www.pandaomeng.com/2019/01-04-gitalk-comment/

### 7）侧边栏设置返回顶部，并且显示百分比

1. 修改主题配置文件

   sidebar 配置下：

   ```
   # Scroll percent label in b2t button.
     scrollpercent: true
   ```

### 8）头像设置

打开 **主题配置文件** 找到`Sidebar Avatar`字段

```
# Sidebar Avatar
avatar: /images/header.jpg
```

这是头像的路径，只需把你的头像命名为`header.jpg`（随便命名）放入`themes/next/source/images`中，将`avatar`的路径名改成你的头像名就OK啦！

### 9）设置"阅读全文"

无需修改配置，你只需要在你的文章想要分隔的地方加上

```
<!--more-->
```

就可以了

![](https://images.pandaomeng.com/0bfb740025c3c3bed7569f5d542b1bfa.jpg)

### 10）去掉文章目录标题的自动编号

打开 **主题配置文件** 找到 `toc` 字段，将number改为false

```
toc:
  enable: true

  # Automatically add list number to toc.
  number: false

  # If true, all words will placed on next lines if header width longer then sidebar width.
  wrap: false
```

效果图：

![](https://images.pandaomeng.com/bdbeeb05a7b3675feb26b6567aeee366.jpg)