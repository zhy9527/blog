---
title: 在vue中应用weui
date: 2019-05-16 17:15:37
tags: js
comments: true
categories: "js"
---

在vue中使用weui的两种方式

## cdn形式引入
这种方式直接在html引入即可
``` html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<meta name="viewport" content="width=device-width, initial-scale=1.0, minimum-scale=1.0, maximum-scale=1.0, user-scalable=no">
		<title>SFOPEN</title>
		<link rel="shortcut icon" href="favicon.ico" />
		<link rel="stylesheet" href="https://res.wx.qq.com/open/libs/weui/1.1.3/weui.min.css">
		<script type="text/javascript" src="https://res.wx.qq.com/open/libs/weuijs/1.1.4/weui.min.js"></script>
		<style>
			.weui-dialog__btn_primary{color: #1131fe}
		</style>
	</head>

	<body>
		<div id="app"></div>
		<!-- built files will be auto injected -->
	</body>

</html>
```
## 在本地引入
有些情况不适用cdn的形式引入，就需要下载到本地引入
``` javascript
// main.js
// 引入weui文件，并且把weui对象绑定到window对象上，方便全局使用
import './comm/css/weui.min.css'
window.weui = require('./comm/js/weui.min.js').weui
```
