---
title: 'npx的使用'
date: 2021-11-08
categories:
- [项目扩展]
- [npm]
tags:
- npx
---
>主要是想解决调用项目内部安装的模块

###安装
**npm install -g npx**
###调用项目安装的模块
假如项目内部安装了webpack，`npm i -D webpack`
一般调用的话只能在项目脚本和package.json的script字段中，如果想在命令行调用需输入`node-module/.bin/webpack --xx`,而npx就是想解决这个问题让项目内部安装的模块用起来更方便，比如刚刚命令改写为`npx webpack -xx`
###原理
就是运行的时候会到node-module/.bin路径和环境变量$PATH里面检查命令是否存在，所以系统命令也可以调用**bash内置命令不能用**
###避免全局安装模块
`npx create-vue-app myProject`,npx将create-vue-app下载到一个临时目录，使用后再删除，所以以后再次执行时会重新下载
npx 还可以执行github上面的`模块源码`,远程代码必须是一个模块即包含package.json和入口脚本


