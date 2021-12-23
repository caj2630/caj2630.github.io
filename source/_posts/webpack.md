title: 'webpack'
date: 2021-12-21
categories:
- webpack
tags:
- webpack
---

##webpack编译流程
 >运行流程是串行的过程
* 初始化：启动构建、读取和合并配置参数，实例化compiler（全局只有一个，包含了完整的webpack配置，负责文件监听和启动编译），执行配置文件中的插件加载plugin（在构建过程中的特定时机会广播出对应的事件，插件可以监听到这些事件的发生，在特定的时机做出对应的事情），执行对象的run方法开始编译；
* 编译：从entry入口解析，针对每个模块串行调用对应的loader，再找到模块依赖的模块，递归的进行编译处理直到所有入口依赖文件都处理完，得到每个被翻译后的模块及它们之间的关系；
* 输出：对编译后的模块组合成chunk,把chunk转换成文件，把文件内容写入到文件系统。

>module 就是没有被编译之前的代码，通过 webpack 的根据文件引用关系生成 chunk 文件，webpack 处理好 chunk 文件后，生成运行在浏览器中的代码 bundle

###hash、chunkhash、contenthash
hash：所有文件哈希值相同，只要改变内容跟之前的不一致，所有哈希值都改变，没有做到缓存意义
chunkhash：根据不同的入口文件(Entry)进行依赖文件解析、构建对应的 chunk，生成对应的哈希值，多个入口时改变一个chunk的内容另一个chunk的hash值不会被改变。css、js的hash值相同，无论是修改css还是jshash值都会改变。
js设置chunkhash,css设置contenthash，css改变则css和js的hash值都会改变，改变js则css被缓存hash值不会被改变
css设置chunkhash，js设置contenthash，css改变则js被缓存hash值不会改变，js改变了css和js的hash值都会改变，
contenthash：css修改内容不会影响到js的hash值，js同理
```ecmascript 6
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
module.exports = {
    mode: "production",
    entry: {
        index: "./src/index.js",
        chunk1: "./src/chunk1.js"
    },
    output: {
        filename: "[name].[contenthash].js"
    },
    module: {
        rules: [
            {
                test: /\.css$/,
                use: [
                    MiniCssExtractPlugin.loader,
                    "css-loader"
                ]
            }
        ]
    },
    plugins: [
        new MiniCssExtractPlugin({
            filename: "[name].[chunkhash].css"
        })
    ]
};

```

`library:{type:'umd'}打包类型`
>commonjs引入
引入import _ from 'lodash';
 
>ts引入
 引入import * as _ from 'lodash';

 如果想用commonjs语法引用则需要在tsconfig.json中配置"allowSyntheticDefaultImports": true 和 "esModuleInterop": true

延伸：
> node模块系统遵循的是commonjs规范

exports 和 module.exports(文件实际导出的是module.exports指向的内存，exports是为module.exports添加属性)
```js
exports = module.exports = {}

// a文件
exports.a = 2
exports = 3
console.log(module.exports, exports) // {a:2} 3

// b文件
let a = require('./a.js')
console.log(a) // {a:2}
```
> ES中的导入导出
export 和 export default

```ecmascript 6
// a.js
export const age = 18
function getName () {
    return 'zs'
}
export { getName }
export const getGender = function () {
    return '男'
}
const height = 180
export default height

// b.js
import {age, getName, getGender} from "./a.js"

import height from "./a.js"

import * as person from "./a.js"
person.height // error, as整合了整个对象，export default是导出了defaul属性
person.default // 180
```




















