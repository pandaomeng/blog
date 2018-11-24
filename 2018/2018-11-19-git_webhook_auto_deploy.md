---
title: githook_auto_deploy
date: 2018-11-19
tag: [git]
---

# 使用webhook，实现自动化部署

###  一、在github上配置webhook

在如下路径做如下配置:

![](http://images.pandaomeng.com/c5cabbce5e9512c2ca52159767b00fe4.jpg)

解释：

- Payload URL：触发git事件时，请求的接口路径，下面我们会用node写对应的controller
- Secret：填写的密码，写controller接口的时候用到
- Just the push event：这里我们只响应push事件



### 二、在自己的服务器上编写响应的接口

前提：服务器上有node环境

1. 创建项目路径

   ```shell
   mkdir -r /home/ameng/webhook
   ```

2. 初始化为node项目

   ```shell
   cd /home/ameng/webhook
   npm init
   ```

   一路回车后，项目就创建完毕了

3. 安装需要的依赖

   ```shell
   npm i github-webhook-handler
   ```

4. 编辑index.js

   ```shell
   vim index.js
   ```

   ```js
   var http = require('http');
   var spawn = require('child_process').spawn;
   var createHandler = require('github-webhook-handler');
   // 下面填写的myscrect跟github webhooks配置一样，下一步会说；path是我们访问的路径
   var handler = createHandler({ path: '/blog_push', secret: 'Zxcvbnm123' });
   http.createServer(function (req, res) {
     handler(req, res, function (err) {
       res.statusCode = 404;
       res.end('no such location');
     })
   }).listen(7777);
   handler.on('error', function (err) {
     console.error('Error:', err.message)
   });
   // 监听到push事件的时候执行我们的自动化脚本
   handler.on('push', function (event) {
     console.log('Received a push event for %s to %s',
       event.payload.repository.name,
       event.payload.ref);
     	runCommand('sh', ['./run.sh'], function( txt ){
       console.log(txt);
     });
   });
   function runCommand( cmd, args, callback ){
       var child = spawn( cmd, args );
       var resp = '';
       child.stdout.on('data', function( buffer ){ resp += buffer.toString(); });
       child.stdout.on('end', function(){ callback( resp ) });
   }
   ```

5. 编写测试用脚本

   ```
   vim run.sh
   ```

   ```
   #!/bin/bash
   
   filename="`date \"+%Y-%m-%d %H:%M:%S\"`.test"
   touch "$filename"
   ```

6. 启动项目

   ```shell
   node index.js &
   ```

7. 配置nginx

   编辑文件 `/etc/nginx/sites-enabled/webhook`

   ```shell
   server {
           listen 80;
           server_name webhook.pandaomeng.com;
   
           location / {
               index  index.html index.htm;
               proxy_pass http://localhost:7777;
               proxy_set_header X-Real-IP $remote_addr;
               client_max_body_size 100m;
           }
   }
   ```

   重启nginx

   ```shell
   nginx -s reload
   ```

























