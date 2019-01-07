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

### 1) 开启一些基础配置

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



