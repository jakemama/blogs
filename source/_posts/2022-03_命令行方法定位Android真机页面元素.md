---
title: 命令行方法定位Android真机页面元素
tags:
  - Android
categories:
  - 前端
date: 2022-03-28 16:17:19
---
## 背景

Android SDK自带的 uiautomatorviewer.bat无法使用，如何拿到调试过程中的页面元素进行定位？

<!--more-->
## 工具准备

保证adb命令可以使用

## 操作过程

打开命令行，输入

```
adb shell uiautomator dump /sdcard/sc.uix
```

输入下列命令，将uix文件保存到当前目录（可以更换目录）

```
adb pull /sdcard/sc.uix
```

输入下列命令，可以在文件夹中打开当前目录，找到保存的uix文件，将文件后缀uix改为xml

```
open ./
```

打开xml文件（可以使用xcode或者sublime）可以得到未格式化的元素结构代码

![未格式化的元素结构代码](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/23d0cd31ecf74792888cec92f461593d~tplv-k3u1fbpfcp-zoom-1.image)

可以选择复制，找一个网上xml格式化工具（比如菜鸟的）

![格式化的元素结构代码](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4985a4ca0a954b4a881ed076aaf90c97~tplv-k3u1fbpfcp-zoom-1.image)

之后可以找自己需要元素的id或者text了

## 总结

Android版本更新之后，uiautomatorviewer.bat就好像不太能用了，上述命令行代码其实就是uiautomatorviewer.bat的实现原理。果然图形化还是靠不住啊，命令行永远的神！