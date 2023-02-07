---
title: vue init webpack project 超时报错解决方案
tags:
  - webpack
  - vue
categories:
  - 前端
date: 2023-02-06 18:57:12
---

## 报错代码：
初始化脚手架时命令：

```javascript
vue init webpack-simple project
```
错误代码如下：
```javascript
vue-cli · Failed to download repo vuejs-templates/webpack: connect ETIMEDOUT 192.30.253.112:443

```
<!--more-->
### 原因分析
与github网络连接不畅，可以尝试下列方法证明：

> 打开终端（cmd）
> 输入命令：ping 192.30.253.112 发现连接超时；
> 输入命令：ping github.com 显示超时
说明连接有问题

### 解决办法
**1.可尝试更换网络或电脑，或者为npm设置代理。**

> 我发现即使在同一网络下，不同电脑的反应不一样，有些可以直接初始化而不报错，有时候换个网络也可以下载。而且有时候电脑可以访问github但是还是无法下载成功。我初步分析认为可能npm也需要设置代理
（设置方法：[为 NPM/Git/Bower 设置代理](http://huangtengfei.com/2015/03/set-proxy-for-npm-and-git-and-bower/)）
这种方法对我有点太专业了，我怕设置出错没有采用，有兴趣的可以看一下

**2.更改host文件，打通网络**

> 详细步骤：[vue init webpack-simple project 报错处理 (connect ETIMEDOUT
> 192.30.253.112)](https://www.cnblogs.com/taiyanghua0522/p/6043942.html)
> 
> 既然网络不通就给他打通，不过对于我来说还是不起作用。（从小到大凡是用host文件来修改bug的方法我从来都没成功过。。。。）

**3.自己下载模板进行离线初始化（亲测可用）**

> 在 [vuejs-templates 的github地址](https://github.com/vuejs-templates/)下载 webpack-simple 或 webpack。（推荐webpack）

如何下载：

> 比如：到[https://github.com/vuejs-templates/webpack](https://github.com/vuejs-templates/webpack)点 Clone or Download 里的 Download ZIP



解压 zip，放置文件夹

> 把解压出的文件夹放在放在项目同级

用相对路径来初始化

```javascript
vue init ./webpack-develop myproject 
```
搞定
![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ca615ca3404440ec8d99a1d8a765ffb9~tplv-k3u1fbpfcp-zoom-1.image)
md这个bug我都搞一下午了，结果最后还是用本地方法解决的，唉气死偶了。