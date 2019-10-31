---
title: jQuery on() click事件在iphone上失效的解决办法
date: 2019-010-06 18:15:37
tags: css
comments: true
categories: css
---
******************
> 每逢秋去冬来是人去花又别，叹一声缘分不该如此难求  <<青衣>>


## 前言

### 问题
页面上有一动态添加的元素，用jq的on()添加click事件，在iphone上无效，但在浏览器和安卓上没问题。
```
$(document).on('click', '#btn', function () {
   alert('Hello！')
})
```


### 解决方法
给该元素添加如下样式；
对于点击的对象，拥有cursor:pointer;这个样式，即鼠标放上去，能够出现“手”型的图标才能视为可以使用点击事件。
```
#btn {
  cursor:pointer
}
```