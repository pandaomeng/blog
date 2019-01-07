---
title: 使用dcoker部署可迁移服务器
date: 2018-07-29
tag: [docker]
---

1. 安装docker

2. pull ubuntu镜像

   ```
   docker pull ubuntu
   ```

3. 启动ubuntu镜像

   ```
   docker run --name  myubuntu -v /root:/root -p 80:80 -p 443:443 -it ubuntu /bin/bash
   ```

   <!--more-->

4. 此时在容器中了，下载nginx并启动

   ```
   apt-get update
   apt-get install nginx
   service nginx start
   ```

   ![nginx运行成功](https://images.pandaomeng.com/169fcd3bdebbc6c95a1f4f7acc9b5bf3.jpg)

5. 安装git

   ```
   apt-get install git
   ```

6. 安装node

   ```
   apt-get install wget
   wget https://nodejs.org/dist/v8.11.3/node-v8.11.3-linux-x64.tar.xz
   apt-get install xz-utils
   xz -d node-v8.11.3-linux-x64.tar.xz
   tar -xvf node-v8.11.3-linux-x64.tar
   mv node-v8.11.3-linux-x64 node
   rm -f node-v8.11.3-linux-x64.tar
   mv node /usr/local/
   ln -s /usr/local/node/bin/node /usr/local/bin/node
   ln -s /usr/local/node/bin/npm /usr/local/bin/npm
   ```

7. 安装hexo

   ```
   npm install -g hexo-cli
   ln -s /usr/local/node/bin/hexo /usr/local/bin/hexo
   cd ~
   hexo init myhexo --no-clone
   hexo g
   ```

   ps: --no-clone是因为clone太慢了

8. 部署hexo

   ```
   rm -rf /var/www/html
   cp -r ~/myhexo/public /var/www/html
   ```

9. 配置git

   ```
   git config --global user.name "pandaomeng"
   git config --global user.email "806636588@qq.com"
   ssh-keygen
   cat ~/.ssh/id_rsa.pub
   ```

10. 进入myhexo的source目录，配置自己的博客地址

  ```
  cd ~/myhexo/source/_posts
  git init
  git remote add origin git@github.com:pandaomeng/blog.git
  git checkout master
  ```

  重新部署

  ```
  rm -rf /var/www/html
  cp -r ~/myhexo/public /var/www/html
  ```
