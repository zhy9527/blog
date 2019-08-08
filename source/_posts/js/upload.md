---
title: 文件上传的2中方式，包含图片压缩
date: 2019-04-30 17:15:37
tags: js
comments: true
categories: "js"
---

代码摘自自己项目，项目框架是VUE，这里只说明功能实现，具体知识点请去MDN查看

## 图片转base64上传
``` html
<input type="file" name="pic" id="pic" class="upload-btn" @change="uploadImg($event,'front')" accept="image/*" />
```
看下面代码，我们先看下FileReader对象：
> [FileReader，查看介绍](https://developer.mozilla.org/zh-CN/docs/Web/API/FileReader)，
1. FileReader 异步读取存储在用户计算机上的文件
2. 当 FileReader 读取文件的方式为  readAsArrayBuffer, readAsBinaryString, readAsDataURL 或者 readAsText 的时候，会触发一个 load 事件。从而可以使用  FileReader.onload 属性对该事件进行处理。
3. 这里会用到load属性 和readAsDataURL()方法

``` javascript
uploadImg(e,type) {
  let that = this
  let file = e.target.files[0]
  let reader = new FileReader()     
  reader.readAsDataURL(file)
  reader.onload = function(e) {     // 当 FileReader 读取文件的方式为  readAsArrayBuffer, readAsBinaryString, readAsDataURL 或者 readAsText 的时候，会触发一个 load 事件
      console.log(this.result) // 生成base64
  }
}
```

## 运用formData文件流上传
``` html
<input type="file" name="pic" id="pic" class="upload-btn" @change="uploadImg($event,'front')" accept="image/*" />
```
这里会用到URL对象和formData对象，包含了图片压缩，代码注释中会说明一些自己的理解，若有偏颇，欢迎指正
1. URL对象相关内容请[移步这里](https://developer.mozilla.org/zh-CN/docs/Web/API/URL)
2. formData对象请[移步这里](https://developer.mozilla.org/zh-CN/docs/Web/API/FormData/Using_FormData_Objects),formData我的理解是可以用ajax形式模拟form表单发送数据
3. 图片压缩我们单独一个模块说明

``` javascript
// 上传身份证图片
  uploadImg(e,type) {
    let that = this
    let file = e.target.files[0]
    lrz(file,{quality: 0.6,width: 800})
    .then(function (rst) {
        // 压缩成功会执行，这里的rst是压缩后图片的结果，我看了下大概压缩率能达到70%，10MB可以压缩到2MB
        console.log(rst);
        if (type == 'front'){
          that.frontImgUrl = URL.createObjectURL(rst.file)    // 这里我的理解是生成了一个图片的url
          that.frontImg = rst.file    // 这是要发送给后端的数据
        } else {
          that.backImgUrl = URL.createObjectURL(rst.file)
          that.backImg = rst.file
        }
    })
    .catch(function (err) {
        // 处理失败会执行
        console.log(err)
    })
  },
  submit() {
    let that = this
    // 创建FormData对象，并且把要传给后端的参数append进去
    let formdata = new FormData();
        formdata.append('id',patient.id);
        formdata.append('frontImg',this.frontImg);
        formdata.append('backImg',this.backImg);
        // Fetch是我封装的axios，这里可以忽略，这里重要的就是组装formdata数据
        Fetch({
          url: "/api/xxxxx",
          method: "post",
          data: formdata
        }).then(response => {
            that.$router.push("aaa");
        }).catch(error => {
          console.log(error);
        });
  }
```

## 图片压缩
1. 图片压缩应该是图片上传必然会遇到的问题，如果图片过大，上传慢且容易失败。我们遇到的问题是图片太大后端无法响应。
2. 因为以前用过这个压缩库，所以这次拿出来说下，也比较简单
3. 图片压缩代码github地址[请点击](https://github.com/think2011/localResizeIMG)
4. 参数配置, [这里也讲的很清楚，不在赘述](https://github.com/think2011/localResizeIMG/wiki/2.-%E5%8F%82%E6%95%B0%E6%96%87%E6%A1%A3)

> 
``` javascript
lrz(this.files[0],{
  // 这里会涉及到
})
.then(function (rst) {
    // 处理成功会执行
    console.log(rst);
})
.catch(function (err) {
    // 处理失败会执行
})
.always(function () {
    // 不管是成功失败，都会执行
});
```