---
title: 使用docker运行一个lamp环境
date: 2018-05-30
tag: [docker]
---

## 执行步骤

1. 拉取lamp镜像

   ```
   docker search lamp
   ```

   本文挑选 fauria/lamp

   ```
   docker pull fauria/lamp
   ```

   <!--more-->

2. 运行容器：

   ```
   docker run --name cocos2d-h5 -it -p 1234:80 -v /Users/pandaomeng/cocos2d/cocos2d-js-v3.13-lite:/var/www/html fauria/lamp  /bin/bash
   ```

   执行完命令后会进入到容器内部。

3. 打开容器的apache服务

   通过下列命令查看apache2的运行状态：

   ```
   service apache2 status
   ```

   开启apache2：

   ```
   service apache2 start
   ```

4. 将容器挂起

   方法：按住control键，依次按p键和q键



##  docker其他常用指令

- docker ps 查看当前在运行的容器
- docker ps -a 查看所有容器
- docker start 启动容器
- docker exec -it containterID /bin/bash 进入正在运行的容器内部
- docker search lamp 搜索所有的lamp镜像

