---
uuid: 7
title: 浅聊ES
categories:
- js
tags:
- Es5
- Es6
---
### 箭头函数
> 写法比普通函数简洁，**let fn = () => {}**

与普通函数区别：
1. 没有this，通过查找作用域链确定
2. 不能使用new关键字调用
3. 没有arguments参数
4. 没有原型
5. 没有super

### weakMap
弱引用，不存在遍历操作（因为不知道什么时候被垃圾回收机制回收），只有set() get() has() delete()等方法。

### Promise
回调/地狱回调















