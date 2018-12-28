---
title: linux 安装 node 环境
date: 2018-12-04
tag: [node]
---

## linux，Mac安装node

作为一名前端工程师，安装node环境是开发的第一步

在node官网 https://nodejs.org/en/download/ 找到node 的 Linux Binaries (x86/x64) 发行版，右键复制它的链接。

（举个栗子：https://nodejs.org/dist/v8.12.0/node-v8.12.0-linux-x64.tar.xz）

1. 下载解压并且重命名

   ```shell
   cd ~
   wget https://nodejs.org/dist/v8.12.0/node-v8.12.0-linux-x64.tar.xz
   xz -d node-v8.12.0-linux-x64.tar.xz
   tar -xvf node-v8.12.0-linux-x64.tar
   mv node-v8.12.0-linux-x64 /usr/local/node
   rm -f node-v8.12.0-linux-x64.tar
   ```

2. 检查是否安装成功

   ```shell
   /usr/local/node/bin/node -v
   ```

3. 为node npm设置软链，放到usr/bin下

   ```shell
   ln -s /usr/local/node/bin/node /usr/bin/
   ln -s /usr/local/node/bin/npm  /usr/bin/
   ```
