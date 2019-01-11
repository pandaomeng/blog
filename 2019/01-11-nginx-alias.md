---
title: nginx 配置alias
date: 2019-01-11
tag: [nginx]
---

## Nginx 配置 alias和root

1. 配置root：

   ```
   location /static {
       root /home/ameng/static/;
   }
   ```

   如果访问 `http://xxx.com/static/test.png`，则会解析到

   `/home/ameng/static/static/test.png`

   root 指令 会把url后的路径全部拼接到root声明的路径后。

2. 配置alias

   ```
   location /static {
       alias /home/ameng/static/;
   }
   ```

   如果访问`http://xxx.com/static/test.png`，则会解析到

   `/home/ameng/static/test.png`

   alias 指令 会忽略url中 location 匹配到的部分，再拼接到申明的路径后面

   即：它将 /test.png 拼接到了/home/ameng/static/ 后

PS：参考文章 https://www.jianshu.com/p/4be0d5882ec5

