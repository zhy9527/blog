---
title: iphone上input不易聚焦
date: 2019-06-30 14:15:37
tags: css
comments: true
categories: "css"
---

## iphone上input不易聚焦

父元素加个`-webkit-user-select:text;`才有效，单独子元素即使加`-webkit-user-select:text`也无效
``` html
<div  style="-webkit-user-select:text;">
    <input type="text" placeholder="请输入用户名" />
</div>
```

```	markdown
> [user-select CSS属性，控制着用户能否选中文本。](https://developer.mozilla.org/zh-CN/docs/Web/CSS/user-select)
```

## 语法
user-select有以下几个值：

* none
元素及其子元素的文本不可选中。 请注意这个Selection 对象可以包含这些元素。 从Firefox 21开始， none 表现的像 -moz-none，因此可以使用 -moz-user-select: text 在子元素上重新启用选择。
* auto
auto的计算值确定如下：

在 ::before 和 ::after 伪元素上，计算属性是 none
如果元素是可编辑元素，则计算值是 contain
否则，如果此元素的父元素的 user-select 的计算值为 all, 计算值则为 all
否则，如果此元素的父级上的 user-select 的计算值为 none, 计算值则为 none
否则，计算值则为 text
* text
用户可以选择文本。
* -moz-none 
元素和子元素的文本将显示为无法选择它们。 请注意， Selection 对象可以包含这些元素。 可以使用 -moz-user-select: text. 在子元素上重新启用选择。 从Firefox 21开始，none 表现得像 -moz-none.
* all
在一个HTML编辑器中，当双击子元素或者上下文时，那么包含该子元素的最顶层元素也会被选中。
* contain
  element (IE-specific alias)
允许选择在元素内开始; 但是，选择将包含在该元素的边界内。 仅在Internet Explorer中受支持。

