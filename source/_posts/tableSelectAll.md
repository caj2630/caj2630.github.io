---
uuid: 5
title: 'elementUI中表格多选实现跨页多选'
---
## element框架当前页多选

  1.手动添加一个el-table-column，设type属性为selection即可
  2. table Events：
    select（参数：selection选中项数组；row行当前选择对象），
    select-all（全选操作， 参数：selection）
    Table Methods：
    toggleRowSelection 切换某一行的选中状态，如果使用了第二个参数则是设置这一行选中与否（参数：row，selected为true则是选中）

## element框架跨页多选

  参数同上
  思路：变量dataList存放当前页数据
        变量arrayList存放选中的row数据
        勾选CheckBox时将所有的row值push到选中的arrayList数组中
        关键一步就是将有重复的值splice包含本身（不单是去重）
      需注意：选中的toggleRowSelection方法可能会出现默认渲染不上，可能是请求的数据比选中的渲染数据渲染慢
  代码实现：

  ```bash
  this.$nextTick(() => {
    this.checked(val)
  })
  checked(row) {
    # searchTable 定义在table上的ref值 等待数据请求完毕再选中
    this.$refs.searchTable.toggleRowSelection(row, true)
  }
  # 去掉重复的并删除自身的函数方法
  delRepeatFun: function (arr) {
    const newArr = [...arr]
    const indexArr = []
    arr.forEach((s, i) => {
      let count = 0
      newArr.forEach((m, j) => {
        if (arr[i].realname === newArr[j].realname && arr[i].userId === newArr[j].userId) { // 当属性值都相同时，打印索引位置
          count++
        }
      })
      // console.log("索引:",i,"相同次数:",count)
      if (count > 1) { // 内层循环结束，当count>1,说明此索引为对象是重复的
        indexArr.push(i)
      }
    })
    // console.log(indexArr) //相同对象索引数组
    let flag = -1
    for (var i = 0; i < indexArr.length; i++) { // 删除一次，索引位
      flag++
      if (flag === 0) {
        newArr.splice(indexArr[i], 1)
      } else {
        newArr.splice(indexArr[i] - flag, 1) // 每次删除，需要删除的索引位就要减去1
      }
    }
    return newArr
  }
  selectChange: function (rows, row)
    # 将所有的操作过的值push进去 然后在delRepeatFun方法中处理重复的值
    this.arrayList = this.arrayList.concat([], row || rows)
    # 取消全选因为其返回值只有选中的 所以这时候rows是空数组 需要将选中值和当前列表值遍历拿到当前列表的所有值从arrayList里面splice
    if (!rows.length) { // 全选取消勾选
      this.dataList.forEach(item => {
        this.arrayList.forEach((ele, index) => {
          if (ele.userId === item.userId) {
            this.arrayList.splice(index, 1)
          }
        })
      })
    }
    this.arrayList = this.delRepeatFun(this.arrayList)
    # this.arrayList 即为选中的数组
  }
  ```
