---
uuid: 1
title: 搭建静态博客
---

## 新建仓库

``` bash
  github 新建一个仓库，仓库名必须为 <user-name>.github.io 格式，其中 <user-name> 是你 github 的昵称
```

More info: [Github](https://github.com)

## 全局安装hexo

### 需要的环境：node git hexo

More info: [Hexo](https://hexo.io/zh-cn/)

``` bash
  npm install -g hexo
```

## 初始化项目

``` bash
  初始化：hexo init <项目名称>
  运行：hexo s
```

## 部署到github

``` bash
  在项目根目录下找到 _congif.yml，找到 deploy 字段并填写完整
    deploy:
      type: git
      repo: <你的仓库地址> https://caj2630.github.io/
      branch: master
  但是我们需要额外的一个工具来帮助我们推到仓库上 hexo-deployer-git
    npm install hexo-deployer-git --save
  执行下面两个命令，就可以把项目自动部署到 github 上
    hexo clean
    hexo deploy
```

## 查看效果

``` bash
  浏览器访问：https://caj2630.github.io/ 即可看到效果。
```
