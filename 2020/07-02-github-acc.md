---
title: git 加速方式
date: 2020-07-02
tag: [git]
---

# git 加速

## 在本地拥有代理的情况下，配置命令行 git 加速

### 一、配置 .ssh/config 文件

配置 http 代理监听 127.0.0.1:1087

![](https://images.pandaomeng.com/20210328020217.png)

```shell
vim ~/.ssh/config
```

添加文件内容如下：

```shell
Host github.com
    User                    git
    ProxyCommand            nc -x localhost:1086 %h %p
```

即可走 http 代理，效果如下

![](https://images.pandaomeng.com/20210328020328.png)

### 二、配置 git 走 socks5

配置 socks5 代理监听 127.0.0.1:1086

![](https://images.pandaomeng.com/20210328020627.png)

在命令行中执行以下命令

```shell
git config --global http.proxy "socks5h://localhost:1086"
git config --global https.proxy "socks5h://localhost:1086"
```

即可。

