---
title: '关于node中自定义命令的看法'
date: 2021-11-20
categories:
- [脚手架]
- [自定义命令]
tags:
- 自定义命令
---
## nodejs环境下的命令行参数解析工具
### yargs
传送门：[https://github.com/yargs/yargs](https://github.com/yargs/yargs)
```javascript
#!/usr/bin/env node
// process.argv(node的参数)
const yargs = require('yargs/yargs')
const { hideBin } = require('yargs/helpers')
const cli = hideBin(process.argv)
const argv = yargs(hideBin(process.argv)).argv
const context = {
    testVersion: pkg.version
}

if (argv.ships > 3 && argv.distance < 53.5) {
  console.log('Plunder more riffiwobbles!')
} else {
  console.log('Retreat from the xupptumblers!')
}

yargs(cli)
.strict() // 严格模式 如果有无法识别的参数将会给出提示
.usage('xx') // 使用--help打印出来的使用信息
.demandCommand(1, '最少输入一个参数') // 最少输入的参数
.alias('h', 'help') // 设置别名
.wrap(cli.terminalWith()) // 当前窗口的宽度
.epilogue('结尾的话')
.options('name', {}) // 也可以是对象({'name': {}})
.group(['debug'], 'dev options') // 设置分组
.recommendCommands() // 根据当前输入的命令找最相似的进行提示
.fail((err,msg) => {xxx}) // 自定义错误信息
.command(
    /**
     * 也可以是对象{
     *     command: 'list',
     *     aliases: ['ls', 'la', 'll'],
     *     describe:'list的描述',
     *     builder: (yargs) => {}
     *     handler: (argv) => {}
     * }
     */
    'init [name]', // 命令名称
    '初始化命令', // 描述
    (yargs) => { // builder函数
        yargs.option('name', {
            type: 'string',
            describe: 'init的option',
            alias: 'n'
        })
    },
    (argv) => { // handler函数
        console.log(argv, 111111111)
    }
)
.parse(argv, context) // 解析命令参数，合并传入的参数
```

### commander



### meow
