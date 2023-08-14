---
title: export与module.export
layout: tweet
icon: balloon
tags:
  - JavaScript
date: 2023-08-14 23:32:00
---

初始化时候，module.exports和exports指向同一个对象。

如果没有对二者进行单独的对象赋值，则二者相等。否则不等。

require()导入的时候，永远导入的是module.exports指向的对象

![](https://ant-blogs-img.oss-cn-beijing.aliyuncs.com/img/20230814233618.png)