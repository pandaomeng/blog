---
title: 使用 Let's Encrypt 注册 ssl 证书
date: 2020-03-24
tag: [ssl, https]
---

# 使用 Let's Encrypt 注册 ssl 证书

本文使用环境

ubuntu 16.04

## Let's Encrypt 简介

> 为了在您的网站上启用 HTTPS，您需要从证书颁发机构（CA）获取证书（一种文件）。Let’s Encrypt 是一个证书颁发机构（CA）。要从 Let’s Encrypt 获取您网站域名的证书，您必须证明您对域名的实际控制权。您可以在您的 Web 主机上运行使用 [ACME 协议](https://tools.ietf.org/html/rfc8555)的软件来获取 Let’s Encrypt 证书。-- 摘自 letsencrypt.org

## Cerbot 简介

> Certbot is a free, open source software tool for automatically using [Let’s Encrypt](https://letsencrypt.org/) certificates on manually-administrated websites to enable HTTPS.
>
> Certbot is made by the [Electronic Frontier Foundation (EFF)](https://www.eff.org/), a 501(c)3 nonprofit based in San Francisco, CA, that defends digital privacy, free speech, and innovation. -- 摘自 cerbot 官网 certbot.eff.org

[Certbot](https://certbot.eff.org/) 是Let’s Encrypt官方推荐的获取证书的客户端，可以帮我们获取免费的Let’s Encrypt 证书。

## 安装 cerbot

```
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update
sudo apt-get install python-certbot-apache
```

## 申请证书 & 配置

```
certbot certonly --webroot -d pandaomeng.com -d www.pandaomeng.com
```

证书生成完毕后，我们可以在 `/etc/letsencrypt/live/` 目录下看到对应域名的文件夹，里面存放了指向证书的一些快捷方式。

这时候我们的第一生成证书已经完成了，接下来就是配置我们的web服务器，启用HTTPS。

![](http://images.pandaomeng.com/20210312143111.png)

配置 web 服务器，启用 nginx

```
server {
        listen 80;
        listen [::]:80;

        rewrite ^(.*)$    https://$host$1    permanent;
        
        server_name pandaomeng.com www.pandaomeng.com;
}

server {
        listen 443 ssl;
        ssl_certificate /etc/letsencrypt/live/pandaomeng.com/fullchain.pem;
        ssl_certificate_key /etc/letsencrypt/live/pandaomeng.com/privkey.pem;
        root /var/www/html;

        index index.html index.htm index.nginx-debian.html;

        server_name pandaomeng.com www.pandaomeng.com;

        if ($host != 'www.pandaomeng.com' ) {
                rewrite ^/(.*)$ https://www.pandaomeng.com/$1 permanent;
        }

        location / {
                try_files $uri $uri/ =404;
        }
}
```

## 自动更新 SSL 证书

Let's Encrypt 证书 90天过期，我们使用 linux 系统自带的 cron 来配置定时任务，在证书即将到期的时候运行命令自动更新证书

执行

```
crontab -e
```

添加以下内容

```
* */12 * * * certbot renew --quiet --renew-hook "/etc/init.d/nginx reload"
```

它表示，每 12 小时，系统会执行一次  certbot renew 的命令，执行成功之后重启 nginx

至此，没费 SSL 证书配置完成。

## 其他

如果使用七牛的图床，可以使用七牛云的免费证书

![](https://images.pandaomeng.com/20210312150634.png)

