---
title: 浏览器DevTools使用技巧
tags:
  - 效率
categories:
  - 前端
date: 2023-11-06 11:46:48
---
分享常见的浏览器控制台使用技巧

# Network选项标签

## 模拟网速

F12 DevTools -> Network -> Simulate network，默认是 No throttling，可以调整为3G、2G网络，还可以自定义延时

## 禁用缓存

F12 DevTools -> Network -> Disable cache，勾选关闭缓存，方便调试

## 截屏

DevTools还提供了获取过程截屏的能力，用户可以直观地看到页面的渲染过程。切换至Network面板，点击Setting按钮后勾选Capture screenshots复选框，这样在刷新页面的同时浏览器会自动把关键帧的截屏保留下来

![](https://ant-blogs-img.oss-cn-beijing.aliyuncs.com/img/20231106121150.png)

<!--more-->

