---
title: 使用dcoker快速启动nginx
date: 2018-07-27
tag: [docker]
---

## 创建nginx挂载目录

```
mkdir -p ~/nginx/html ~/nginx/logs ~/nginx/conf
cd ~/nginx/
touch html/index.html
touch conf/default.conf
echo 'hello world' > html/index.html
```

命令的参数：

- **-p** (--parents)：可以是一个路径名称。此时若路径中的某些目录尚不存在，加上此选项后,系统将自动建立好那些尚不存在的目录，即一次可以建立多个目录;
- **www**: 目录将映射为 nginx 容器配置的虚拟目录。
- **conf**: 目录里的配置文件将映射为 nginx 容器的配置文件。

<!--more-->

### 编辑 default.conf 如下：

```
vim ~/nginx/conf/default.conf
```

```
server {
    listen       80;
    server_name  localhost;

    #charset koi8-r;
    #access_log  /var/log/nginx/host.access.log  main;

    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
    }

    #error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

    # proxy the PHP scripts to Apache listening on 127.0.0.1:80
    #
    #location ~ \.php$ {
    #    proxy_pass   http://127.0.0.1;
    #}

    # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
    #
    #location ~ \.php$ {
    #    root           html;
    #    fastcgi_pass   127.0.0.1:9000;
    #    fastcgi_index  index.php;
    #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
    #    include        fastcgi_params;
    #}

    # deny access to .htaccess files, if Apache's document root
    # concurs with nginx's one
    #
    #location ~ /\.ht {
    #    deny  all;
    #}
}
```



## 运行nginx镜像

```
docker container run -d --name mynginx -p 80:80 -v $PWD/html:/usr/share/nginx/html -v $PWD/conf:/etc/nginx/conf.d nginx
```

