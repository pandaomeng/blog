---
title: 记录使用nginx过程中遇到的一些问题和解决方法
date: 2019-05-11
tag: [nginx]
---

# 记录使用nginx过程中遇到的一些问题和解决方法

## 403 Forbidden

系统ubuntu

修改文件`/etc/nginx/nginx.conf`

```
vim /etc/nginx/nginx.conf
```

将第一行的

```
user www-data;
```

改为

```
user root;
```

重启nginx

```
nginx -s reload
```

