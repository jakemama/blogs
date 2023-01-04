---
title: mongodump备份数据记录
tags:
  - bug
  - 笔记
categories:
  - 后端
date: 2023-01-04 16:23:58
---
最近在学习mongodb，希望可以在自己的云服务器上创建自己的数据库。将mongo镜像部署完成之后，一切也都运行正常。但是mongo镜像的默认的数据存储位置我找不到，就想着用mongodump来实现数据备份，这样也方便后期导出和迁移，尽量和具体的mongo镜像解耦。在这过程中遇到了很多问题，在此记录一下。

<!--more-->

## mongdb版本过高

现在直接试用docker pull mongo:latest下载下来的最新版本已经是v6.0.0+了，和之前v4+有一些区别。首先是v6+取消了mongod命令，改用mongsh。所以进入mongo镜像，MongoDB 6.0 及以上版本使用以下命令：

```
docker exec -it mongo mongosh admin
```

旧版本使用

```
docker exec -it mongo mongo admin
```

最开始我使用的是v6+版本，但是在运行mongodump命令时报错了：Unsupported OP_QUERY command: getnonce。The client driver may require an upgrade.

这是因为mongodb本身是由两部分组成的 [driver version and server version](https://community.openhab.org/t/mongodb-server-version-the-client-driver-may-require-an-upgrade/138362)。

mongodb v6+采用的client driver好像是v3.9的（猜测，没有找到client driver版本）

根据官方文档，client driver 3.9 只能适配 mongodb server versions 2.6 - 4.0.

所以最新版本v6+对目前的client driver来说版本太高了。所以要么对client driver进行升级，要么对mongodb进行降级

不过我没有找到具体升级client driver的方法，所以决定将mongodb降级到v4.2.23


## mongodump版本过低

结果我再次运行mongodump命令的时候，又报错了：assertion: 2 { ok: 0.0, errmsg: "Auth mechanism not specified", code: 2, codeName: "BadValue", operationTime: Timestamp 1573815888000|1, $clusterTime: { clusterTime: Timestamp 1573815888000|1,

这次是因为mongodump 版本较低导致的，需要卸默认yum源安装版本版本，重新安装新版本。

系统默认安装版本：2.x
需求版本（支持集群版本）：4.x

运行了 mongodump --version 果然是2.6.1版本

不过参考文章中[mongodump 报错:errmsg: "Auth mechanism not specified](https://blog.csdn.net/kjh2007abc/article/details/103104731)的这个升级方式没看懂，我就搜了一下mongodump的升级方式，发现果然[从4.4版本开始不再自动跟随数据库一起安装，而是需要自己手动安装](https://blog.csdn.net/weixin_44799217/article/details/127940551?spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1-127940551-blog-110732889.pc_relevant_multi_platform_whitelistv3&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1-127940551-blog-110732889.pc_relevant_multi_platform_whitelistv3)。（但我寻思我这版本4.2.3也没到4.4啊）

但是问题不大，反正现在用的2.6.1版本肯定太低了，根据上面这篇文章的参考方式，需要手动下载新版database-tools压缩包，解压之后这里面会有新版的mongodump，再将这个mongodump替换你现在的mongodump就行了。

首先通过wget下载对应的压缩包，你可以到[官网目录](https://www.mongodb.com/try/download/database-tools)挑选最新的版本

```
wget https://fastdl.mongodb.org/tools/db/mongodb-database-tools-rhel70-x86_64-100.6.1.tgz
```

下载完压缩包并解压，可以看到解压出来的文件夹里面有一个mongodump应用，接下来通过which mongodump来找到当前mongodump所在的文件路径
```bash
which mongodump // /usr/bin/mongodump

// 将当前目录下的mongodump复制到目标路径，就可以自动覆盖
cp mongodump /usr/bin/

//这个时候在使用mongodump --version，就是最新的了
mongodump --version
// mongodump version: 100.6.1
// git version: 6d9d341edd33b892a2ded7bae529b0e2a96aae01
// Go version: go1.17.10
//  os: linux
//  arch: amd64
//  compiler: gc

```

## mongodump权限不足

再次运行mongodump命令，又报错了：Failed: error connecting to db server: server returned error on SASL authentication step: Authentication failed

还好这个解决比较简单，应该是因为权限不足，在命令后面加上--authenticationDatabase admin就行

```
mongodump -h 127.0.0.1 --port 27017 -d test -u admin -p 123456 -o /root/mongo/db/data --authenticationDatabase admin
```

大功告成，撒花

## 参考
1. [mongodb版本过高，client driver不支持](https://community.openhab.org/t/mongodb-server-version-the-client-driver-may-require-an-upgrade/138362)
2. [mongodump 报错:errmsg: "Auth mechanism not specified](https://blog.csdn.net/kjh2007abc/article/details/103104731)
3. [mongodump工具安装及使用详解](https://blog.csdn.net/weixin_44799217/article/details/127940551?spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1-127940551-blog-110732889.pc_relevant_multi_platform_whitelistv3&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1-127940551-blog-110732889.pc_relevant_multi_platform_whitelistv3)
4. 