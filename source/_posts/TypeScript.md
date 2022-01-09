---
title: '初识TypeScript'
date: 2022-01-06
categories:
- [项目扩展]
- [TypeScript]
- [js]
tags:
- typescript
---

### TypeScript定义
>TypeScript是javascript的一个超集，静态类型语言，是类型检查工具。
> js程序的静态类型检查工具，编译时期以静态检查的方式保证程序中的类型都是正确的。

### 为什么要使用TypeScript
> 1 使得程序更容易理解
> 2 ide提示，更高的编程效率
> 3 更少的bug
> 4 非常好的包容js
> 5 提高开发者编程水平

### TypeScript类型

    基础类型：number string null undefined boolean void symbol emun
    对象类型：对象类型 数组类型 类类型 函数类型

### type和interface区别

    type可以声明基本类型别名、联合类型、元祖等类型，interface不行
    interface可以声明合并，type不行

### class类
访问类型：

    private：私有的，仅供内部使用
    public：公共的，内部和外部都可使用
    protected：受保护的，仅内部和继承的子类使用
构造函数：

    关键字：`constructor`
    如果不写的话默认为空 constructor (){}
    如果子类继承父类并使用了构造函数则子类必须使用关键字`super`

### 泛型

函数中使用
```typescript
    function join<T>(first:T) {
        return first
    }
    join<string>('1')
// 如果是数组参数
    function fn<T>(params: Array<T>) {
        return params
    }
    fn<string>(['1', '2'])
```
类中使用

```typescript
// ====改造前====
class SelectStudent {
    constructor(private student: string[] | number[]) {
    }

    getStudent(index: number): string | number {
        return this.student[index]
    }
}

const selectStudent = new SelectStudent(['zs', 'ls', 'wu'])
selectStudent.getStudent(1) // ls

// ====改造后====
class SelectStudent<T> {
    constructor(private student: T[]) {
    }
    getStudent(index: number): T {
        return this.student[index]
    }
}
const selectStudent1 = new SelectStudent<string>(['zs', 'ls', 'wu'])
selectStudent1.getStudent(1) // ls

// 泛型继承
interface student {
    name: string
}
class SelectStudent<T extends student> {
    constructor(private student: T[]) {
    }
    getStudent(index: number): string { // 对应接口中的字段
        return this.student[index].name
    }
}
const selectStudent2 = new SelectStudent([
    {name: 'zs'},{name: 'ls'},{name: 'wu'}
])
selectStudent2.getStudent(1) // ls

// 泛型约束
class SelectStudent<T extends string | number> { // 约束泛型，要么是字符串要么是数字
    constructor(private student: T[]) {
    }
    getStudent(index: number): T {
        return this.student[index]
    }
}
```
### 命名空间namespace
>减少全局变量污染