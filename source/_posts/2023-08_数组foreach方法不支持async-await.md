---
title: 数组foreach方法不支持async/await
layout: tweet
icon: balloon
tags:
  - JavaScript
date: 2023-08-02 18:20:51
---

javaScript的数组方法foreach不支持异步，因为源码中没有对传入的异步函数进行处理。所以不要使用async/await。改用for循环