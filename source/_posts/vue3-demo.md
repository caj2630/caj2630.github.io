---
uuid: 5
title: 'vue3.0初了解'
date: 2021-05-01
categories:
- [项目扩展]
- [vue]
tags:
- vue3.0
---

### 安装

    使用vue-cli安装最新稳定版：npm install vue@next；
    使用vite构建工具：npm init @vitejs/app <project-name>

![Image text](./images/vue3.0/vue3-cli-package.jpg)

### 开始使用

![Image text](./images/vue3.0/catalog.jpg)

相比vue2.0 目录结构未发生变化

main.js(3.0版本明显精简很多，按需引入.)

vue2.0
![Image text](./images/vue3.0/main-js(2.0).jpg)
vue3.0
![Image text](./images/vue3.0/main-js(3.0).jpg)

>vue3.0

1.多节点

2.反应数据reactive data包含在反应状态reactive state变量中

    引入reactive，在reactive方法中声明，setup方法返回变量，template中获取返回对象中的键值对
3.methods 编写和反应数据一样需要在setup中返回

4.生命周期函数也需要和反应数据一样引入，在setup中使用

5.computed和反应数据一样，需要在reactive中使用 xxx：computed(() => xxx = xx)

6.props this不能直接拿到prop属性

```js
setup(props, context){ // 接收两个参数prop不可变组件参数，context vue暴露出来的参数（emit，slots，attrs）
    // 通过props参数获取属性
}
```
>proxy

对目标对象操作之前提供拦截，可以对外界的操作进行操作，可以不直接操作对象本身而是通过操作代理对象来间接操作对象达到目的。

>vue2.x

响应式规则：
    对象：递归循环vue的每个属性，给每个属性添加getter和setter，当属性发生变化时更新视图
    数组：重写数组方法，调用数组方法时会触发更新也会对数组中的每一项进行监听
