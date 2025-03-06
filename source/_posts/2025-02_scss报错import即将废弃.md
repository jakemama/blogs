---
title: scss报错Sass @import rules are deprecated and will be removed in Dart Sass 3.0.0
layout: tweet
icon: balloon
categories:
  - 前端
tags:
  - css
  - vue
date: 2025-02-25 18:57:50
---

原因：安装的sass和sass-loader版本过高，进行降级处理

坑：删掉node_modules文件夹，重新安装时会默认安装高版本，即使你在package.json里面写了sass和sass-loader的版本号

查看sass版本方法：进入node_modules文件夹，找到sass依赖，查看package.json中的版本号

解决办法：输入命令来单独安装指定版本

```
npm i sass@1.62.1
npm i sass-loader@10 -D
```