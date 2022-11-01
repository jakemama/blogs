---
title: packagejson配置解析
tags:
  - 笔记
categories:
  - 前端
date: 2022-10-31 15:37:20
---
package.json字段解析

<!--more-->
```json
{
  "types": "index.d.ts",    // 指向 TypeScript 的入口文件，比如说 index.d.ts。有时候你会发现别人在使用 typings，这两者作用相同。
  "main": "index.common.js",  // 指向支持 CommonJS 模块的入口文件，比如说 index.common.js。这个文件名是自己打包文件的时候配置的，只需要该文件及其依赖文件支持 CommonJS 模块即可
  "module": "index.esm.js", // 指向支持 ESM 模块的入口文件，比如说 index.esm.js，同样该文件名也是自定义的。
  "browser": "index.js", // 指向支持 UMD 模块的入口文件，通常是 index.js，同样该文件名也是自定义的。
  /**
    这个字段很实用，已经被很多开源库拿来使用了。当打包工具支持该字段时，上文的四个字段都会被忽略，当然前提是我们在 exports 中做了相应的配置。
    该字段的作用分为两部分：
    • 可以用来配置哪些文件是可以被用户导入的
    • 可以根据不同的条件来配置哪些文件被导入。比如说可以根据当前环境来区分被导入的文件，开发环境时使用未压缩的文件，正式环境时反之
  */
  "exports": {
    ".": {
      // 用户 import 使用时
      "import": {
        // 根据环境引入不同的文件
        "node": "./dist/vue.runtime.mjs",
        "default": "./dist/vue.runtime.esm.js"
      },
      // 除了 require 对应 main 字段之外，其它都一样
      "module": "index.esm.js",
      "browser": "index.umd.js",
      "require": "index.common.js",
      "types": "index.d.ts"
    },
    // 用户可以导入 dist 文件夹下的文件
    "./dist/*": "./dist/*",
 },
 /*
  首先这个字段是被 Webpack 使用的，作用是协助 Webpack 进行 tree shaking。
  多数情况下我们可以直接设置值为 false，这样 Webpack 就会自动帮助我们删除未被 import 的代码，但是在一些情况下不能这样设置。比如说：
  • 是否会对包之外的对象进行修改，比如说在 window 上挂载属性等等行为
  • 是否在包内 import 了 CSS 文件，尤其对于组件库而言。如有此行为并且将 sideEffects 设置为了 false，那么用户使用组件库的时候就会出现样式丢失的情况，因为 Webpack 在打包过程中没有把 CSS 文件包含进来
 */
  "sideEffects": false,
  "packageManager": "yarn@2.0.0", // 包管理，可以使项目开发人员统一使用yarn
}
```