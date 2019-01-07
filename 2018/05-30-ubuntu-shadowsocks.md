---
title: 使用Ubuntu搭建Shadowsocks（小飞机）
date: 2018-05-30
tag: [shadowsocks]
---

## 购买一台海外的vps

1. 我这里选择 vultr https://www.vultr.com

   部署一台ubuntu服务器，在本地测试是否ping通

   ```
   ping ***.**.**.***
   ```

2. 通过putty连上服务器

   <!--more-->

## 配置服务器端ss

安装python pip

```
sudo apt-get update
sudo apt-get install python-pip
sudo apt-get install python-setuptools m2crypto
```

安装shadowsocks

```
pip install shadowsocks
```

创建 shadowsocks.json

```
touch ~/shadowsocks.json
```

shadowsocks.json内容：

```
{
  "server": "你的服务器ip",
  "server_port": 2333,
  "local_address": "127.0.0.1",
  "local_port": "1080",
  "password": "你的密码",
  "timeout": "300",
  "method": "aes-256-cfb",
  "fast_open": false,
  "workers": 1,
  "prefer_ipv6": false
}
```

执行下列命令启动shadowsocks:

```
ssserver -c shadowsocks.json -d start
```

- 执行这一步的时候可能会报以下错误

  ```
  undefined symbol: EVP_CIPHER_CTX_cleanup
  ```

  不要急，只需要执行以下两行命令即可

  ```shell
  vim /usr/local/lib/python2.7/dist-packages/shadowsocks/crypto/openssl.py
  ```

  在vim的底线命令模式（Last line mode）中，执行下列命令将cleanup替换为reset

  ```shell
  :%s/cleanup/reset/g
  ```

  保存退出后重新启动shadowsocks即可



至此，服务器端shadowsocks就配置完毕了。

## 下载并配置客户端

window地址：

https://github.com/shadowsocks/shadowsocks-windows/releases

mac地址：

https://github.com/shadowsocks/ShadowsocksX-NG/releases



## 进阶：配置加速脚本

如果你上面的步骤都完成了，那么恭喜你，接下来可以配置加速脚本，极速观看youtube 1080p 视频啦。

懒癌终极命令：

```shell
wget https://raw.githubusercontent.com/teddysun/across/master/bbr.sh && chmod 755 bbr.sh && ./bbr.sh
```

推荐一个git仓库，上面有好多加速脚本：

https://github.com/teddysun/across



