---
title: 阿里云服务器从零开始部署博客
date: 2018-09-26
tag: [linux, nginx]
---

# 阿里云服务器从零开始部署静态博客

##配置快捷登录

在阿里云的控制台中复制服务器的公网IP： 47.101.21.142 （下面的命令都改为你自己的IP）

在本地执行命令，并根据提示输入密码：

```shell
ssh-copy-id root@47.101.21.142
```

之后连接服务器就不需要密码了

只需要输入

```shell
ssh root@47.101.21.142
```

即可免密登录

但是登录服务器还要记住公网ip，不开心，于是

```shell
vim ~/.ssh/config
```

在文件中输入（记得先按一下 i 键开启编辑模式哦~）

```shell
Host aliyun
HostName 47.101.21.142
User root
IdentitiesOnly yes
```

保存退出后，即可使用如下命令登录啦

```shell
ssh aliyun
```

<!--more-->

### 给服务器改名

登录后我们发现服务器实例名是一串很奇怪的字符串（就像这样 iZuf64a6e4zw2jzsp0ovjhZ）

我们编辑/etc/hostname 文件

```
vim /etc/hostname
```

将 iZuf64a6e4zw2jzsp0ovjhZ 改为 aliyun，保存退出后，重启服务器生效

```
reboot
```

(大概15s后就重启完毕啦)，然后我们就发现名字修改成功了。



## 常用工具及软件安装

##安装Git

```shell
apt-get update
apt-get install git
```

生成公钥 密钥，并且在github上配置

```
ssh-keygen -t rsa -C "1281732005@qq.com"
cat ~/.ssh/id_rsa.pub
```

将输出的公钥添加到github的settings -> SSH and GPG keys -> SSH keys 里面

## 安装Node

作为一名前端工程师，我已经迫不及待要先安装node了

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

## 安装Nginx

```shell
apt-get install nginx
```

安装完 直接就可以在 47.101.21.142 查看nginx的欢迎页面了

nginx 默认的站点配置的位置在 /etc/nginx/sites-enabled

PS: 记得在阿里云后台配置安全组策略，netstat -ant |grep 80 查看80端口的监听状况

##安装Hexo

博客，我来啦~

```
npm install -g hexo-cli
ln -s /usr/local/node/bin/hexo /usr/local/bin/hexo
cd ~
sudo hexo init myhexo
cd myhexo
hexo g
rm -rf /var/www/html/
cp -r ~/myhexo/public/ /var/www/html
```

此时博客已经部署完成了

## 配置Git仓库

```
cd ~/myhexo/source
rm -rf _posts
git clone git@github.com:pandaomeng/blog.git _posts
cd ~/myhexo
hexo g
rm -rf /var/www/html/
cp -r ~/myhexo/public/ /var/www/html
```

OK啦

## 配置Hexo主题

主题NexT文档

https://theme-next.iissnan.com/getting-started.html

```
git clone https://github.com/iissnan/hexo-theme-next themes/next
```

修改主题配置文件

```
vim _config.yml
```

设置 theme: next

```
vim themes/next/_config.yml
```

设置 scheme: Pisces

```
cd ~/myhexo
hexo g
rm -rf /var/www/html/
cp -r ~/myhexo/public/ /var/www/html
```

完成啦~







