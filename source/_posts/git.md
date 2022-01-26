---
uuid: 10
title: git
categories:
- 工具
tags:
- git
---
### git格式
> commit 的类别

feat：新功能（feature）
fix：修补bug
docs：文档（documentation）
style： 格式（不影响代码运行的变动）
refactor：重构（即不是新增功能，也不是修改bug的代码变动）
test：增加测试
chore：构建过程或辅助工具的变动

>changelog.md文件中

type 为 feat 和 fix ，则该 commit 将肯定出现在 Change log 之中。

>Standard Version

安装：npm i standard-version
使用：
```
    "scripts": {
        "release": "standard-version"
    }
```
脚手架入口package.json文件中："bin": "bin/cli.js",
使用
    yargs命令解析工具
    semver语义版本解析器
    conventional-changelog从 git 元数据生成变更日志
    conventional-changelog-config-spec描述由传统配置支持的上游工具配置选项的规范
    conventional-changelog-conventionalcommits用于自动 CHANGELOG 生成和版本管理的规范的具体实现
    conventional-recommended-bump根据常规提交获得推荐的版本提升。
