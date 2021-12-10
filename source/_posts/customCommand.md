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
**支持自动生成help内容，必填参数有检查校验**
**定义较为复杂**

Yargs 框架通过使用 Node.js 构建功能全面的命令行应用，它能轻松配置命令，解析多个参数，并设置快捷方式等，还能自动生成帮助菜单。

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
**支持自动生成help内容，主要通过方法调用**
**required不做检查，需自己校验检查参数到底有没有传**

传送门：[https://github.com/tj/commander.js/blob/master/Readme_zh-CN.md](https://github.com/tj/commander.js/blob/master/Readme_zh-CN.md)

安装 `npm install commander`
```javascript
#!/usr/bin/env node

const { Command } = require('commander');
const program = new Command();

program
    .version('0.0.1', '-v', 'version') // 默认-V
    .option('-c, --cheese <type>', 'add the specified type of cheese', 'blue') // 短标志，自定义标志，选项描述（可省略），默认值（可省略）
    .requiredOption('-c, --cheese <type>', 'pizza must have cheese') // 必填选项,必填选项要么设有默认值，要么必须在命令行中输入，对应的属性字段在解析时必定会有赋值
    .parse(process.argv)

console.log(`cheese: ${program.opts().cheese}`);
```



### meow
**简单，结构设计较好**
**不支持自动生成help内容，需手动写**
