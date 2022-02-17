---
title: 浅谈vue操作
date: 2020-11-16
categories: 
- [项目扩展]
- [vue]
tags:
- Vue
---

## 为什么使用vue.js
1. 体积小
2. 组件化开发：单页面，使用小型 独立和通常可复用的组件构成大型应用
3. 只关注视图层，数据驱动视图，双向数据绑定
4. 虚拟dom，频繁操作dom造成浏览器刷新卡顿
5. 容易上手，社区活跃

### vue源码分析
> 本质上是一个function实现的class，在他的原型及本身都扩展了一系列方法和属性。

#### 数据驱动
一个核心思想: 数据驱动。视图由数据驱动生成。


### 组件传值
一：父传子

  1.props

二：子传父

  1.$emit（"事件名", 值（可多个））

  2.$parent/$children $ref

三：兄弟传值

  1.vuex

  2.$emit和props组合

  3.EventBus(创建一个EventBus.js文件，在需要的组件中引入/main.js文件引入，传值：event.$emit("事件名", 值)，接收：event.$on("事件名", (值)=>{}))
```js
// EventBus.js 暴露vue实例
import Vue from "Vue"
export default new Vue()
```
四：多层父子组件通信

  1.vuex

  2.provide/inject

```js
// 方法一：不能实时更新
// 祖/父组件
provide: {
    name: 'zs'
}
// 子组件
inject: ['name']

// 方法二：挂载太多多余的属性
// 祖/父组件
provide () {
    return {
        parentObj: this // 将整个实例挂载
    }
}
// 子组件 parentObj包含了传入的所有实例this的值
inject: ['parentObj']

```


## 引言
  
  ![Image text](images/error.png)
  处理方案：
    原因在于js本身值传递的特点导致，v-for最开始是每个item都有自己的key但是对象。数组是按引用传递的，内存地址不变 因为data数据改变但是key不变所以内存里就会有相同的key，可以采用JSON.stringfy（）或者先清空再赋值方法

### 加不加key区别

  Vue默认使用就地更新策略，如果顺序被改变vue将不会移动dom元素匹配数据项，而是就地更新每个元素，并且确保它们在每个索引位置正确渲染。（默认模式是高效的，但只适用于不依赖子组件状态或临时DOM状态的列表渲染输出）

  对于列表的增删如果给列表的每一项增加key即唯一索引，就可以清楚的知道两个列表之间的变化，不加key的话只能一个个对比然后进行操作。

### key的作用

  更新组件时判断两个节点是否相同， 相同就复用否则就删除旧的创建新的,为了给 Vue 一个提示，以便它能跟踪每个节点的身份，从而重用和重新排序现有元素
  因为带唯一key每次更新都不能找到可复用节点，要销毁和创建vnode，在dom中添加移除节点对性能的影响更大，而不带key只是对节点值进行修改 --- 》 对于只是简单的列表输出不带key的性能比带key好
  对于大部分场景来说组件都有自己的状态。需添加key来更新组件状态


