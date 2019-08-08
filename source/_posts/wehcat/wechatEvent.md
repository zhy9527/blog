---
title: 微信小程序阻止事件冒泡
date: 2019-05-30 17:15:37
tags: WeChat
comments: true
categories: "WeChat"
---

## 微信小程序阻止事件冒泡

* 使用 catch 不用bind
bind事件绑定不会阻止冒泡事件向上冒泡，catch事件绑定可以阻止冒泡事件向上冒泡。

如在下边这个例子中，点击 inner view 会先后调用handleTap3和handleTap2(因为tap事件会冒泡到 middle view，而 middle view 阻止了 tap 事件冒泡，不再向父节点传递)，点击 middle view 会触发handleTap2，点击 outer view 会触发handleTap1。
``` javascript
<view id="outer" bindtap="handleTap1">
  outer view
  <view id="middle" catchtap="handleTap2">
    middle view
    <view id="inner" bindtap="handleTap3">
      inner view
    </view>
  </view>
</view
```

* 在方法最后加上return false