---
title: "nginx: [emerg] zero size shared memory zone “perip”"
tags:
  - nginx
categories:
  - 后端
date: 2023-07-24 11:34:08
---
运行环境：centos7

运行`nginx -t`，检查nginx的配置文件是否正确，出现一条报错信息：

```shell
[root@web01 conf.d]# nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: [emerg] zero size shared memory zone "perip"
nginx: configuration file /etc/nginx/nginx.conf test failed
```

提示nginx: [emerg] zero size shared memory zone “one”，名称为one大小的共享内存空间zene没有。
<!--more-->

因为我的nginx配置文件中server块有一条限制ip的规则`limit_conn perip 2;`

```shell
server {
    listen 80;
    server_name localhost;
    limit_conn perip 2;
    location / {
        ...
    }
}
```
## 解决办法

在nginx 配置文件http块中添加一条命令，在http区域创建一个zone内存空间
```shell
http {
  limit_conn_zone $binary_remote_addr zone=perip:10m;
  server{}
  .......
}
```

相当于在内存中创建了一个水桶，这个水桶扎了很多眼用来漏水。`limit_conn perip 2;` 这句直白解释： 我家在非洲，比较穷，我去村里找一个名称叫zone存水的大桶。zero size shared memory zone “perip” 这个错误是 我现在还没找到这个桶，我们村根本不存在这个漏水桶，所以报错了。

同样适合于类似报错的情况`zero size shared memory zone "xxx"`，在http块中声明对应的zone空间