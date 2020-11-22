---
title: about Vue key值问题
type: "tags"
date: 2020-11-16
---
## 引言
  
  ![Image text](../images/error.png)
  处理方案：
    原因在于js本身值传递的特点导致，v-for最开始是每个item都有自己的key但是对象。数组是按引用传递的，内存地址不变 因为data数据改变但是key不变所以内存里就会有相同的key，可以采用JSON.stringfy（）或者先清空再赋值方法

## 浅谈vue中的key作用

### 先聊聊v-for中key作用

  