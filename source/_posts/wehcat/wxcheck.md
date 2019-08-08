---
title: 微信小程序checkbox样式修改
date: 2019-04-30 17:15:37
tags: weChat
comments: true
categories: "weChat"
---
> [原文](https://www.jianshu.com/p/f8d7932006c0)

## 微信小程序checkbox样式修改
``` css
/*checkbox 整体大小  */
checkbox {
  width: 240rpx;
  height: 90rpx;
}
/*checkbox 选项框大小  */
checkbox .wx-checkbox-input {
  width: 50rpx;
  height: 50rpx;
}
/*checkbox选中后样式  */
checkbox .wx-checkbox-input.wx-checkbox-input-checked {
  background: #FF525C;
}
/*checkbox选中后图标样式  */
checkbox .wx-checkbox-input.wx-checkbox-input-checked::before {
  width: 28rpx;
  height: 28rpx;
  line-height: 28rpx;
  text-align: center;
  font-size: 22rpx;
  color: #fff;
  background: transparent;
  transform: translate(-50%, -50%) scale(1);
  -webkit-transform: translate(-50%, -50%) scale(1);
}
```

## 效果如下
![你想输入的替代文字](./img/wxcheck.png)