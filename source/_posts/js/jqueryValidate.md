---
title: jQuery Validate 自定义验证方法的实现 
date: 2019-09-05 17:15:37
tags: js
comments: true
categories: "js"
---

最近做后台验证，jQuery Validate 提供的无法满足，所以需要写自定义验证方法，以前也有写，但是没记录

## 表单部分
``` html
<script src="/lib/jquery.js"></script>
<script src="/dist/jquery.validate.min.js"></script>
<script src="/dist/localization/messages_zh.js"></script>
<form class="cmxform" id="commentForm" method="get" action="">
    <fieldset>
        <legend>请输入姓名</legend>
        <p>
            <label for="password">password </label>
            <input id="password" name="password"  type="password" required>
        </p>
        <p>
            <input class="submit" type="submit" value="Submit">
        </p>
    </fieldset>
</form>
```
下面是js验证部分

``` javascript
  $.validator.setDefaults({
      submitHandler: function () {
          console.log("提交成功")
      }
  });
  
  $().ready(function () {
      $("#commentForm").validate({
          onkeyup: function(element) { $(element).valid(); },     // 首次监听表单数据变化，触发验证
          rules:{
              password: {
                  coustom: ['a','f'] 
              }
          }
      });
      
      // 密码验证
      $.validator.addMethod("coustom", function(value, element) {   
          var tel = /^(?![a-zA-Z]+$)(?![A-Z0-9]+$)(?![A-Z\W_!@#$%^&*`~()-+=]+$)(?![a-z0-9]+$)(?![a-z\W_!@#$%^&*`~()-+=]+$)(?![0-9\W_!@#$%^&*`~()-+=]+$)[a-zA-Z0-9\W_!@#$%^&*`~()-+=]{8,16}$/;   // 必须包含大小写字母、数字且为8-16个字符
          return this.optional(element) || (tel.test(value));
      }, "必须包含大小写字母、数字且为8-16个字符");
  });
```
