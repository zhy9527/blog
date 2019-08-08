---
title: 自定义复选框+全选/全不选
date: 2019-04-30 17:15:37
tags: js
comments: true
categories: "js"
---

最近在做一个自定义复选框+全选/全不选功能，项目基于vue，现在整理如下

大致思路
> 1. 声明一个空的数组checked，数据为list
2. 选中：把选中的项的id push进checked;
3. 取消选中：判断当前id是否在checked中，若存在删除当前id
4. 全选功能: 遍历数组，取到所有id，push进checked
5. 全不选：checked重置为空数组
6. 全选按钮状态：判断checked数组的长度是否和数据list长度一致，若一致则为全选状态，则给class加active
7. 复选框状态：用indexOf是否大于-1判断当前id是否存在checked中，若大于-1，则给class加active

## 自定义全选功能代码
``` html
<!-- 全选按钮 -->
<div
  class="check all"
  :class="list.length == currChecked.length ?'active':''"
  @click="checkBoxAll"
>全选</div>
<!-- 复选框 -->
<div
  class="check"
  :class="currChecked.indexOf(item.id) > -1 ? 'active':'a'"
  @click="checkBox(currChecked,item.id)"
></div>
```
``` javascript
// 
data: {
  list: [], // 全部选中的值
  currChecked: []  // 当前已选中的复选框
},
methods: {
  // 点击复选框
  checkBox: function(array, id) {
    if (array.indexOf(id) == -1) {
      array.push(id);
    } else {
      this.remove(array, id);
    }
  },
  // 全选按钮
  checkBoxAll: function() {
    let currChecked = this.currChecked
    let list = this.list
    if (currChecked.length == list.length) {
      currChecked = [];
    } else {
      currChecked = [];
      for (var i = 0; i < list.length; i++) {
        currChecked.push(list[i].id);
      }
    }
    return currChecked;
  },
  // 数组移除值
  remove: function(array, val) {
    var index = array.indexOf(val);
    if (index > -1) {
      array.splice(index, 1);
    }
  }
}
```
