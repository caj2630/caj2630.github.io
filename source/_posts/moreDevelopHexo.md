---
uuid: 8
title: 多人开发hexo博客
categories:
- 博客
- hexo
tags:
- hexo
---
## 前言

``` bash
  1.github添加分支为hexo（随意按照自己习惯来命名）
  2.设置hexo为默认分支
  3.hexo代码从master分支迁移过来
  4.克隆hexo代码
  5.将本地开发代码复制到克隆hexo分支的文件夹中除了_defaultxx文件和npm包文件
  6.git add .   git commit -m 'xx'  git push完成提交
  7.即可实现多人开发博客, 接着就是按照git提交代码格式提交代码。
  (代码提交问题，git支持三种协议git://   ssh://   http:// 本来push的时候是走ssh隧道的但是因为设置了http代理所以就走了http的代理于是提交失败。
  提示：OpenSSL SSL_connect: SSL_ERROR_SYSCALL in connection to github.com:443，
  方法一： 偶尔出现可能是网络问题
  方法二： 使用命令git config --global --unset http.proxy即可取消代理)
```
