---
title: 打造属于你的专属博客，Hexo，Next主题
date: 2019-01-04
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

## 二、配置NexT

参考NexT的官方文档：

https://theme-next.iissnan.com/third-party-services.html

**注意：以下配置就要重新生成后生效（hexo g)**

### 1) 修改一些基础配置

1. 编辑 `themes/next/_config.yml`

   ```git
   -seo: false
   +seo: true
   ```

   ```git
    menu:
      home: / || home
      #about: /about/ || user
   -  #tags: /tags/ || tags
   +  tags: /tags/ || tags
   ```

   ```git
    # Schemes
   -scheme: Muse
   +#scheme: Muse
    #scheme: Mist
   -#scheme: Pisces
   +scheme: Pisces
    #scheme: Gemini
   ```

   ```git
    motion:
   -  enable: true
   +  enable: false
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
   -  enable: false
   +  enable: true
      # custom uv span for the whole site
      site_uv: true
   -  site_uv_header: <i class="fa fa-user"></i>
   +  site_uv_header: <i class="fa fa-user"></i> 访问人数
      site_uv_footer:
      # custom pv span for the whole site
      site_pv: true
   -  site_pv_header: <i class="fa fa-eye"></i>
   -  site_pv_footer:
   +  site_pv_header: <i class="fa fa-eye"></i> 总访问量
   +  site_pv_footer: 次
      # custom pv span for one page only
      page_pv: true
   -  page_pv_header: <i class="fa fa-file-o"></i>
   -  page_pv_footer:
   +  page_pv_header: <i class="fa fa-file-o"></i> 浏览
   +  page_pv_footer: 次
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

