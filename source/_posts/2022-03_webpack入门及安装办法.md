---
title: webpack入门及安装办法
tags:
  - webpack
categories:
  - 前端
date: 2022-03-28 17:19:18
---
## **webpack快速入门**
### 什么是webpack
官方网址：webpackjs.com
自动化打包工具，优化我们的项目，服务请求更快。
webpack是现代JavaScript应用程序的静态打包工具，当webpack处理应用程序时，它会在内部构建一个依赖图，此依赖图会映射项目所需的每个模块，并生成一个或多个bundle包。
<!--more-->

### 为啥要使用webpack
**代码转换**:TypeScript编译成JavaScript，LESS/SCSS编译CSS，ES6/7编译成ES5，虚拟DOM编译成真实DOM等等
**文件优化**:压缩js，css，HTML代码，压缩合并图片，图片base64等
**代码分割**:提取多个页面的公共代码、提取首屏不需要执行部分的代码
**模块合并**:在采用模块化的项目里会有很多个模块，模块下有很多文件，需要构建功能把各个模块文件合并成一个文件
**自动刷新**:监听本地源代码的变化,自动重新构建，刷新浏览器
**代码校验**: Eslint代 码规范校验和检测，单元测试
**自动发布**:自动构建出线上发布版本，传输给发布系统
...........................
### npm：安装webpack前需要了解的
npm = node package manager
NPM的全称是Node Package Manager，是一个NodeJS包管理和分发工具，已经成为了非官方的发布Node模块（包）的标准。
**常用的npm语法：**

```javascript
npm install xxx		//把xxx安装在当前目录
npm i xxx -g		//把xxx安装在全局 global
npm i xxx@xx.xx		//安装制定版本号的xxx	eg：npm i jquery@1.11.3
npm uninstall xxx	-g		//卸载xxx包
```
安装在全局的作用：

 1. 所有的项目都能用
 2. 可以使用命令（在win下）
 3. 安装在全局模块不能基于CommonJs等模块规范在代码中导入模块

### 安装webpack
#### 安装package.json
在根目录下右键，在集成终端中打开，首先我们先安装一个package.json文件。在终端中输入

```javascript
npm init -y		//在当前目录下生成一个package.json文件，注意当前项目目录中不能包括中文或特殊字符
```
要注意npm正如上文所说，要在node环境下才能运行。要提前安装好node（安装教程很多，自行安装）。
每个项目的根目录下面，一般都有一个package.json文件，定义了这个项目所需要的各种模块，以及项目的配置信息（比如名称、版本、许可证等元数据）。npm install命令根据这个配置文件，自动下载所需的模块，也就是配置项目所需的运行和开发环境。

#### 安装webpack

```javascript
npm i webpack webpack-cli --save-dev	//--save-dev 安装在开发环境，不加的话默认安装在生产环境
```
全局安装只要电脑不发生问题，就无需多次安装，但是官方不推荐全局安装 webpack。这会将你项目中的 webpack 锁定到指定版本，并且在使用不同的 webpack 版本的项目中，可能会导致构建失败。

有些时候npm安装会非常非常慢，这时我们可以先安装yarn，利用yarn再安装webpack会快很多

```javascript
npm i yarn -g		//yarn安装
yarn add webpack webpack-cli -D
```

#### 添加webpack.config.js文件
在根目录下创建添加webpack.config.js文件。并添加如下代码：

```javascript
const path = require ('path');

module.exports = {
    //设置编译的入口文件（真实项目中一般开发的代码都会放在src目录中）
    entry:'./src/main.js',      
    //设置编译的出口文件
    output:{
        //编译后文件名
        filename:'bundle.[hash:5].js',        //hash在内容更新后会自动生成一串哈希值，来避免浏览器的强缓存，及时更新文件
                                                //浏览器的强缓存：当运行的文件名相同时，会默认加在缓存里的文件
        //输出的目录（绝对路径）
        path:path.resolve(__dirname, 'build')
            }
             mode:"development"    //开发模式，没有对js等文件压缩，默认生成的是压缩文件
    }
```
#### 配置package.json
打开package.json，将scrtpts下代码换成下面样式


```javascript
"scripts": {
    "serve": "webpack",
  },
```
配置完成后，我们在终端输入的如下命令，可以直接打包。

```javascript
npm run start
```

### 优秀相关博客
自己配完webpack之后就想写篇博客记录一下，方便自己或初学者查询。在准备博客的过程中看了许多相关博客，有一些让我叹为观止，一度很羞愧自己的作品哈哈哈，大家如果看我的不太明白在这里推荐两个：
清晰的webpack及插件安装过程：[webpack安装及打包详细过程](https://www.cnblogs.com/aizai846/p/11497508.html)
webpack安装过程中可能遇到的问题及解决方法：[webpack安装常见问题及基本插件使用](https://www.cnblogs.com/jinzhaozhao/p/10006131.html)



![在这里插入图片描述](https://img-blog.csdnimg.cn/20200826142038903.png#pic_center)
