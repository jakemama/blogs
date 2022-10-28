---
title: charles移动端抓https包
tags:
  - 笔记
  - 网络
categories:
  - 前端
date: 2022-10-28 15:55:09
---
# 1.Mac安装Charles根证书
![](https://ant-blogs-img.oss-cn-beijing.aliyuncs.com/img/20221028160557.png)

<!--more-->

# 2.信任Charles根证书
在钥匙串中找到Charles Proxy CA证书，设置为“始终信任”，这里会让你输入密码，直接填写Mac开机密码即可。
![](https://ant-blogs-img.oss-cn-beijing.aliyuncs.com/img/20221028161519.png)

# 3.Charles设置「 Enable SSL Proxying 」
只有https才需要配置， http请求似乎不配置这个也能抓包
![](https://ant-blogs-img.oss-cn-beijing.aliyuncs.com/img/20221028161635.png)

在弹出框中选中“Enable SSL Proxying”，然后点击Add按钮，在弹出的表单中设置需要抓包的HTTPS的Host(域名)和Port(端口)，如果需要抓取所有HTTPS，则Host填入“*”，Port一般填“443”即可。

![](https://ant-blogs-img.oss-cn-beijing.aliyuncs.com/img/20221028161741.png)

配置Host， 只抓包特定域名的请求，

适用场景：如果连接代理抓包，则无法进行苹果相关操作，比如苹果登录、苹果支付。

这时就可以配置host，只拦截知道的域名请求。 （对于http请求，不管是否配置host都是可以抓包的）

![](https://ant-blogs-img.oss-cn-beijing.aliyuncs.com/img/20221028161928.png)

![](https://ant-blogs-img.oss-cn-beijing.aliyuncs.com/img/20221028161956.png)

# 4.移动设备上安装Charles证书
![](https://ant-blogs-img.oss-cn-beijing.aliyuncs.com/img/20221028162037.png)

![](https://ant-blogs-img.oss-cn-beijing.aliyuncs.com/img/20221028162144.png)

## 4.1 设置网络代理
![](https://ant-blogs-img.oss-cn-beijing.aliyuncs.com/img/20221028162235.png)

## 4.2 下载安装Charles证书，Safari中输入 chls.pro/ssl
![](https://ant-blogs-img.oss-cn-beijing.aliyuncs.com/img/20221028162451.png)

## 4.3 设置信任Charles证书

设置—通用—关于本机—证书信任设置

![](https://ant-blogs-img.oss-cn-beijing.aliyuncs.com/img/20221028162552.png)

# 5.https抓包失败问题
https抓包报错： You may need to configure your browser or application to trust the Charles Root Certificate. See SSL Proxying in the Help menu.

![](https://ant-blogs-img.oss-cn-beijing.aliyuncs.com/img/20221028162627.png)

解决： charles证书未信任，才会导致如上错误。
1、查看Mac有没有信任Charles的根证书，以上第二步；
2、查看移动设备有没有信任Charles证书，以上第四步的步骤3.