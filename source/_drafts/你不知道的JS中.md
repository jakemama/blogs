---
title: 你不知道的JS中
tags:
  - xx
categories:
  - xx
---
## 1.类型
使用typeof判断null值返回“object”，其余基本类型均有同名的字符串值与其对应
``` javascript
typeof null === "object" // true
```
<!--more-->
对于未定义(undeclared)和未声明的变量，typeof均会返回undefined
``` javascript
var a
typeof a; // "undefined"
typeof b; // "undefined"
```
## 2.值
使用`delete`运算符可以数组中元素删除，但是数组的length属性并不会发生变化

稀疏数组：含有空白或空缺单元的数组
``` javascript
var a = []
a[0] = 1
// 此处没有设置a[1]单元
a[2] = 3
a[1]  // undefined
a.length   // 3
```

将类数组转换成数组的两种方法：工具函数slice(...)和ES6内置工具函数Array.form(...)
``` javascript
var arr1 = Array.prototype.slice.call(arguments)

var arr2 = Array.form(arguments)
```

------
JavaScript中的字符串是不可变的，字符串的成员函数不会改变其原始值，而是创建并返回一个新字符串

当需要对字符串进行比较复杂的操作时，通常的思路是将字符串转换成数组进行操作，之后再拼接回字符串
> 上述方法对于包含复杂字符（Unicode，如星号、多字节字符等）的字符串并不适用。建议使用功能更加完备的Unicode的工具库，比如[Esrever](https://github.com/mathiasbynens/esrever)

如果需要经常以字符数组的方式来处理字符串，不如直接用数组，在需要时用join("")来转换为字符串
------
NaN是不是数字的数字，对其进行typeof运算返回"number"，且是唯一一个非自反的值，NaN !== NaN 为true。

内建的全局函数isNaN(...)有个缺陷，对字符串也会返回true。建议使用ES6的 Number.isNaN(...)

ES6中新加入一个工具方法Object.is(...)来判断两个值是否绝对相等，可以用来处理几乎所有特殊情况（NaN比较，0与-0的比较），不过能使用 == 和 === 时就尽量不要用Object.is(...)，因为前者效率更高更通用。
------

## 3.原生函数
字面量定义与构造函数定义相比，语法简单，执行效率也高，会进行预编译和缓存，也能避免大部分意料之外的问题。

## 4.强制类型转换
