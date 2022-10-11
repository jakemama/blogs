---
title: prettier配置文件解析
tags:
  - 笔记
categories:
  - 前端
date: 2022-10-11 10:53:54
---

.prettierrc.json 配置字段解析

<!--more-->

# json 字段解析

```json
{
  "semi": false, // 是否使用分号
  "singleQuote": true, // 使用单引号代替双引号
  "trailingComma": "none" || "all", // 多行时尽可能使用逗号结尾
  "printWidth": 80, //单行长度
  "tabWidth": 2, //缩进长度
  "useTabs": false, //使用空格代替tab缩进
  "quoteProps": "as-needed", //仅在必需时为对象的key添加引号
  "jsxSingleQuote": true, // jsx中使用单引号
  "bracketSpacing": true, //在对象前后添加空格-eg: { foo: bar }
  "jsxBracketSameLine": true, //多属性html标签的‘>’折行放置
  "arrowParens": "always", //单参数箭头函数参数周围使用圆括号-eg: (x) => x
  "requirePragma": false, //无需顶部注释即可格式化
  "insertPragma": false, //在已被preitter格式化的文件顶部加上标注
  "proseWrap": "preserve", //不知道怎么翻译
  "htmlWhitespaceSensitivity": "ignore", //对HTML全局空白不敏感
  "vueIndentScriptAndStyle": false, //不对vue中的script及style标签缩进
  "endOfLine": "lf", //结束行形式
  "embeddedLanguageFormatting": "auto", //对引用代码进行格式化
}
```

# 处理 eslint 相关冲突

使用 Prettier 解决代码格式问题，使用 linters 解决代码质量问题

安装解决冲突需要用到的两个依赖

- eslint-config-prettier 关闭可能与 prettier 冲突的规则
- eslint-plugin-prettier 使用 prettier 代替 eslint 格式化
