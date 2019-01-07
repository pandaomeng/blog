---
title: Ubuntu 16.04 安装mysql
date: 2018-11-09
tag: [linux, mysql]
---

### 准备

- 阿里云服务器
- ubuntu 16.04



### 步骤

1. **安装mysql**

```shell
apt-get update
apt-get install mysql-server mysql-client
```

安装的过程中，会提示你输入密码和确认密码。安装完成后，mysql服务会自动启动。

<!--more--> 

2. **测试是否安装成功**

在终端输入 `mysql -u root -p ` 接下来会提示你输入密码，输入正确密码，即可进入，即为安装成功

3. **修改配置`/etc/mysql/mysql.conf.d/mysqld.cnf`**

找到bind-address，把127.0.0.1 改为0.0.0.0 

修改后，保存。重启mysql服务。 

```shell
service mysql restart
netstat -nlt |grep 3306
```

我们看到从之间的网络监听从 127.0.0.1:3306 变成 0 0.0.0.0:3306，表示MySQL已经允许远程登陆访问。

4. **修改mysql访问权限**

```shell
mysql mysql -u root -p
# 输入密码
use mysql;
update user set host = '%' where user = 'root';
```

通过下面的命令查看是否成功

```shell
select user, host from user
```

修改完后记得再次重启mysql

```shell
service mysql restart
```



### 问题排查

如果远程连接还是失败

1. 通过站长工具 http://tool.chinaz.com/port/ 测试 3306 端口是否开启

![](https://images.pandaomeng.com/bcc723b3e2dd03e6c0d5c21c7e8e4863.jpg)

	如果未开启，可能是阿里云后台的安全组没有放通3306端口

