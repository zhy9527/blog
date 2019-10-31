---
title: IOS12以上微信内置浏览器下键盘收起底部空白的问题
date: 2019-09-30 17:15:37
tags: weChat
comments: true
categories: "weChat"
---
******************

## BUG 表现

Bug表现：
在IOS12以上的系统下，微信打开链接点击输入框获取焦点后虚拟键盘自动弹出，输入内容后收起键盘，原来弹出键盘的位置一片空白，页面没有自动适应整个屏幕。



## 解决方案

在公共js文件下对设备进行判断，如果为IOS设备则全局处理该问题，即在当前页面滚动的位置上下滚动1px的距离即可实现页面的自适应！

```javascript
let ua = window.navigator.userAgent;
if(!!ua.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/)){
    //$alert('ios端');
    document.body.addEventListener('focusout', () => {
        var currentPosition,timer;
        var speed=1;
        timer=setInterval(function(){
            currentPosition=document.documentElement.scrollTop || document.body.scrollTop;
            currentPosition-=speed; 
            window.scrollTo(0,currentPosition);//页面向上滚动
            currentPosition+=speed;
            window.scrollTo(0,currentPosition);//页面向下滚动
            clearInterval(timer);
        },100);
    })
}else if(ua.indexOf('Android') > -1 || ua.indexOf('Adr') > -1) {
    //$alert('android端');
}
```