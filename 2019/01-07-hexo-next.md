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

### 1）接入百度统计

 1. 登录百度站长统计，点击新建站点

	2. 查看统计代码，获取 Baidu Analytics ID

    ![](https://images.pandaomeng.com/0b0bf4f2cde82ccfca87eaca239e448d.jpg)

3. 编辑 `themes/next/_config.yml`

   ![](https://images.pandaomeng.com/f6147aae2be0d46099fa12c8fdce8c50.jpg)![](http://images.pandaomeng.com/17cda1aa809fec9f50d565cac3b9e50a.jpg)

