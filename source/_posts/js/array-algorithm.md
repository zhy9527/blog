---
title: 数组去重，删除数组指定值
date: 2019-04-30 17:15:37
tags: js
comments: true
categories: "js"
---

最近在做一个[自定义复选框+全选/全不选]()功能，涉及到一些数组的处理,自定义复选框

## 数组去重
``` javascript
uniq: function(array) {
  var temp = []; //一个新的临时数组
  for (var i = 0; i < array.length; i++) {
    if (temp.indexOf(array[i]) == -1) {
      temp.push(array[i]);
    }
  }
  return temp;
}
```
``` javascript
// es6中新增的数据结构Set
const set = new Set([1, 2, 3, 4, 4]);
[...set]
// [1, 2, 3, 4]
```


## 删除指定数组
``` javascript
// 数组移除值
remove: function(array, val) {
  var index = array.indexOf(val);
  if (index > -1) {
    array.splice(index, 1);
  }
}
```


## 数组排序
``` javascript
// 数组移除值
var arr = new Array(6)
    arr[0] = "10"
    arr[1] = "5"
    arr[2] = "40"
    arr[3] = "25"
    arr[4] = "1000"
    arr[5] = "1"
console.log(arr.sort((a,b)=>{return b-a}))
```
