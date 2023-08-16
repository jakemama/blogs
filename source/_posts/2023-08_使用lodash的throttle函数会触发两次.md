---
title: 使用lodash的throttle函数会触发两次
tags:
  - bug
  - lodash
categories:
  - 前端
date: 2023-08-16 18:34:30
---
当使用lodash的throttle函数时会触发两次，分别在最开始和最后。

严格来说不算是bug，因为[官方文档](https://www.lodashjs.com/docs/lodash.throttle)写的很清楚。throttle函数其实有三个参数：

`_.throttle(func, [wait=0], [options=])`

`func`: 要节流的函数
`wait`: 等待时间
`options`: 选项
`options.leading=true` (boolean): 指定调用在节流开始前，也就是第一次点击。
`options.trailing=true` (boolean): 指定调用在节流结束后，也就是最后一次点击。

options的默认值为:`{leading: true, trailing: true}`

所以其实throttle函数默认就是会调用两次。分别是第一次和最后一次。

如果想要throttle函数只会调用一次，可以设置options.trailing=false。这样函数的表现就像普通的截流函数了。

```js
// 点击后就调用 `renewToken`，但5分钟内超过1次。
var throttled = _.throttle(renewToken, 300000, { 'trailing': false });
```
