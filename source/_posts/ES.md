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
> this的确定是在定义的时候

与普通函数区别：
1. 没有this，通过查找作用域链确定
2. 不能使用new关键字调用
3. 没有arguments参数
4. 没有原型
5. 没有super


### Map
键值对集合，
### weakMap
弱引用，不存在遍历操作（因为不知道什么时候被垃圾回收机制回收），只有set() get() has() delete()等方法。
### Set（去重）
函数接受一个数组作为参数
### weakSet（去重）
函数接受一个对象作为参数, 只要对象在外部消失了，那么在weakSet中的引用也会自动消失。



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
**Promise.any()**,只要有**一个**fulfilled就会是fulfilled状态，如果**所有**都是rejected的状态才会调用后面方法。如果是rejected失败之后返回的是一个记录所有失败的数组
```js
const resolve = Promise.resolve(1)
const rejected = Promise.reject(-1)
const anotherRejected = Promise.reject(-2)

Promise.any([resolve, rejected, anotherRejected]).then((res) => {
    console.log(res) // 1
})
Promise.any([rejected, anotherRejected]).catch((res) => {
    console.log(res) // [-1, -2]
})
```
Promise.try就是模拟try代码块
```js
Promise.try(() => {})
```

### async和await
注意点： 1. async后面的promise对象可能是rejected结果所以要写在try/catch里面
2. 多个await操作，如果不是继发关系最好同时触发；
```js
    const fn1 = await getName()
    const fn2 = await getAge()
// 改写为
    const [fn1, fn2] = await Promise.all([getName(), getAge()])
```
3. await命令只能用在async函数之中。















