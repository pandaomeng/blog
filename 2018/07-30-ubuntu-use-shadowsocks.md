---
title: ubuntu使用shadowsocks
date: 2018-07-30
tag: [linux, ubuntu, shadowsocks]
---

## 配置shadowsocks

1. 安装shadowsocks

   ```
   apt-get install shadowsocks
   ```

2. 编辑配置文件

   shadowsocks.json:

   ```
   {
     "server": "{your-server}",
     "server_port": 40002,
     "local_port": 1080,
     "password": "{your-password}",
     "timeout": 600,
     "method": "aes-256-cfb"
   }
   ```

3. 启动

   ```
   sudo sslocal -c shawdowsocks.json -d start
   ```

   <!--more-->

## 配置代理

1. 安装polipo

   ```
   sudo apt-get install polipo
   ```

2. 修改他的配置 文件 /etc/polipo/config:

   ```
   sudo vim /etc/polipo/config
   ```

   ```
   logSyslog = true
   logFile = /var/log/polipo/polipo.log
   proxyAddress = "0.0.0.0"
   socksParentProxy = "127.0.0.1:1080"
   socksProxyType = socks5
   chunkHighMark = 50331648
   objectHighMark = 16384
   serverMaxSlots = 64
   serverSlots = 16
   serverSlots1 = 323
   ```

3. 重启polipo服务:

   ```
   service polipo restart
   ```

   如果重启失败则手动重启, 如下

   ```
   ps -ef |grep polipo
   kill -9 xxxxxx
   service polipo start
   ```

4. 为终端配置http代理：

   ```
   export http_proxy="http://127.0.0.1:8123/"
   ```

5. 测试是否成功:

   ```
   curl www.google.com
   ```

   ![](https://images.pandaomeng.com/f0ea757cbabaebe9f766731256cc0303.jpg)

大功告成！

PS: 

- 重启服务器需要重新执行

  ```
  sslocal -c shawdowsocks.json -d start
  export http_proxy="http://127.0.0.1:8123/"
  ```

- git 命令需要在后面跟参数

  ```
  --config http.proxy=localhost:8123
  ```

  举个栗子：

  ```
  git clone xxx --config http.proxy=localhost:8123
  ```
