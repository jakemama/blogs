---
title: '命令行运行提示 zsh: command not found: flutter（MAC环境）'
tags:
  - bug
categories:
  - 前端
date: 2022-03-28 16:24:15
---
如果还没有下载Flutter SDK，请先去官网下载并解压。官网地址：https://flutter.io/sdk-archive/#macos

Flutter 的渠道版本会不停变动，请以 Flutter 官网为准。另外，在中国大陆地区，要想正常获取安装包列表或下载安装包，可能需要翻墙，读者也可以去 Flutter github 项目下去下载安装包，地址：https://github.com/flutter/flutter/releases 。

<!--more-->
如果已经下载了SDK，仍然出现上处错误，一般是因为PATH环境不对，所以命令行找不到flutter命令的文件

-   确定您 Flutter SDK 的目录记为 “FLUTTER_INSTALL_PATH”，您将在后面步骤中用到。（“FLUTTER_INSTALL_PATH”是一个变量，其值为你之前下载的flutter SDK的文件目录
-   打开 (或创建) `$HOME/.bash_profile`。文件路径和文件名可能在你的电脑上不同.
-   添加以下路径:

```
export PATH=[FLUTTER_INSTALL_PATH]/flutter/bin:$PATH
```

-   运行 `source $HOME/.bash_profile` 刷新当前终端窗口
-   **注意:** 如果你使用终端是 zsh，终端启动时 `~/.bash_profile` 将不会被加载，解决办法就是修改 `～/.zshrc` ，在其中添加：source ～/.bash_profile

出现这种报错肯定是环境变量的路径问题或者压根没有安装相关命令代码，按需解决就好

以上～