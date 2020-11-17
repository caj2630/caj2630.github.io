---
uuid: 2
title: 每日一题 来源力扣
---
## 题目描述1

  超市鸡蛋打折，阿姨们买折扣鸡蛋数量不一，现超市搞活动送鸡蛋；
  有些阿姨拿了送的鸡蛋也不一定是拥有最多鸡蛋的人，有的人可以，
  现在将活动鸡蛋送给大家，看看是否手上拥有的鸡蛋高于或等于最高的人，
  能就是拥有最多的否则不是。
  
## 解题思路1

  1.找到最大值
  2.讲自己拥有的和送的相加与最大值做比较
  3.大于return ture否则false

## 代码块1

>>
  1.function (aggs, extraAgges) {
    var max = Math.Max(...aggs) // 最大值
    return aggs.map((ele, index)=>{
      return ele + extraAgges >= max
    })
  }
  2.function (aggs, extraAgges) {
    var max =0
    for (item in aggs) { // 最大值
      max = Math.max(max, item)
    }
    return aggs.map((ele, index)=>{
      aggs[index] = ele + extraAgges >= max
    })
    return aggs[index]
  }
>>

## 题目描述2

  给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。

  你可以假设每种输入只会对应一个答案。但是，数组中同一个元素不能使用两遍。
  eg：nums = [2, 7, 11, 22] target = 9

## 代码块2

>>
  var twoSum = function(nums, target) {
    var map = {}
    var loop = 0
    var dic
    while(loop < nums.length) {
      dic = target - nums[loop]
      if (map[dic] !== undefined) {
        return [map[dic], loop]
      }
      map[nums[loop]] = loop
      loop++
    }
  }
>>

## 题目描述3

  只出现一次的数字
  eg：nums = [4,1,2,1,2] return 4

## 代码块3

>>
 1. 利用indexOf和lastIndexOf方法
  var singleNumber = function(nums) {
    nums.forEach(ele => {
      if (nums.indexOf(ele) !== nums.lastIndexOf(ele)) {
        return ele
      }
    })
  }
 2. 先排序再对比前后是否有相同的
  var singleNumber = function(nums) {
    nums = nums.sort()
    nums.forEach((ele, index) => {
      if (ele !== nums[index - 1] && ele !== nums[index + 1]) {
        return ele
      }
    })
  }
 3. 异或运算符
  var singleNumber = function(nums) {
    var temp = 0
    nums.forEach((ele, index) => {
      temp ^= ele
    })
    return temp
  }

>>
