---
title: '拥有自己的一个脚手架'
date: 2021-11-08
categories:
- [脚手架]
- [vue-cli]
tags:
- 脚手架
---
### 搭建一个脚手架
1. 初始化项目`npm init`
2. package.js中添加 `"bin": {
    "test-cli": "bin/index.js'   
}`, `test-cli表示为此脚手架命令，bin/index.js文件表示为输入命令执行的文件`
3. 新建bin/index.js文件，添加`#!/usr/bin/env node`,`表示寻找环境变量中的node执行`

### npm发布脚手架
1. 需要有一个npm的账号
2. 项目中输入npm login（第一次需要登录，输入账号密码和邮箱）
3. npm publish 即可在npm中查看刚刚发布的包

### 使用刚刚发布的脚手架
1.  npm install -g test-cli,全局环境使用
2.  如果不想发布测试，本地连接测试，在项目中输入npm link，即可将连接指向本地进行调试
3.  解除本地连接，npm unlink test-cli即可解除
