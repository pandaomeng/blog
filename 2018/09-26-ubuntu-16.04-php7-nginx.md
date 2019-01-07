---
title: ubuntu 16.04 安装php7
date: 2018-09-26
tag: [php, nginx]
---

## Ubuntu 安装php7 nginx

1. Ubuntu 16.04官方源自带PHP7，所以可以直接使用apt-get来安装。

   ```shell
   sudo apt-get update
   sudo apt-get install php7.0-fpm php7.0-mysql php7.0-common php7.0-mbstring php7.0-gd php7.0-json php7.0-cli php7.0-curl libapache2-mod-php7.0
   ```

2. 通过命令查看php是否安装成功

   ```shell
   php -v
   ```

   ![](https://images.pandaomeng.com/a7e0df97d7051b4ca900ff3b07385bce.jpg)

   <!--more-->

3. 通过命令查看php7.0-fpm的运行状态

   ```shell
   service php7.0-fpm status
   ```

   ![](https://images.pandaomeng.com/daf8e2114250658f9efb48739fe4b612.jpg)

   php7.0-fpm 的配置文件 `/etc/php/7.0/fpm/pool.d/www.conf`

   ![](http://images.pandaomeng.com/4b37cf5e99ed18843bdcf54f9f66d671.jpg)

   监听的是 /run/php/php7.0-fpm.sock

4. 配置nginx监听到同一个文件，让两者进行通信

   编辑文件 `/etc/nginx/sites-enabled/php`

   ```shell
   server {
           listen 80;
           server_name php.pandaomeng.com;
           access_log   /var/log/nginx/access_php.log;
           error_log    /var/log/nginx/error_php.log;
   
           location / {
                   index index.php;
                   root /home/php;
           }
           location ~ [^/]\.php(/|$) {
                   root /home/php;
                   fastcgi_pass unix:/run/php/php7.0-fpm.sock;
                   fastcgi_index index.php;
                   include fastcgi.conf;
           }
   }
   ```

   重启nginx：

   ```shell
   nginx -s reload
   ```

5. 编辑文件 /home/php/inde.php

   ```
   <?php
           phpinfo();
   ?>
   ```

6. 访问php.pandaomeng.com

   ![](https://images.pandaomeng.com/edba75e2cedb58b0be07c49674a65b2d.jpg)

   大功告成了！

