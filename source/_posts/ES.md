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
优点：用同步操作写法表达异步操作
缺点：一旦触发中途无法停止，处于pending状态时无法得知是刚开始还是要结束，不设置回调函数内部会报错但是不会反应到外部。
**Promise.all()**,当有一个promise实例返回rejected或者都返回fulfilled才会调用all后面的回调
```js
const p1 = new Promise()
const p2 = new Promise()
Promise.all([p1, p2]).then(() => {})
```
**Promise.race()**,只要有一个实例率先改变状态就会调用then后面的方法。
**Promise.allsettled()**,等到所有对象不管是成功还是失败都发生状态变更。返回值格式**[{status: "fulfilled", value: ""} // 成功状态格式, {status: "rejected", reason: ""}// 失败状态格式]**。
**Promise.any()**















