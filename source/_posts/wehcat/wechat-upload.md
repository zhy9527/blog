---
title: 微信小程序多图上传，包含压缩，以base64格式
date: 2019-8-1 19:15:37
tags: WeChat
comments: true
categories: "WeChat"
---
************
## 背景
### 需求：小程序多图上传，转base64上传，后来发现图片太大，上传失败
### 思路：获取原图临时路径，用canvas重绘图片并生成压缩后临时路径，生成base64格式

### 主要方法：
  1. [`wx.chooseImage` 调用微信选择图片api](https://developers.weixin.qq.com/miniprogram/dev/api/media/image/wx.chooseImage.html)
  2. [`wx.getImageInfo` 获取图片详情信息](https://developers.weixin.qq.com/miniprogram/dev/api/media/image/wx.getImageInfo.html)
  3. [`wx.createCanvasContext`  创建canvas](https://developers.weixin.qq.com/miniprogram/dev/api/canvas/wx.createCanvasContext.html)
  4. [`wx.getFileSystemManager().readFile`   生成base64格式图片](https://developers.weixin.qq.com/miniprogram/dev/api/file/wx.getFileSystemManager.html)

效果如下
![效果](https://raw.githubusercontent.com/zhy9527/blog/master/source/_posts/img/wxcheck.png)

## WXML内容
``` html
<view class='content'>
  <view class='img-box'>
    <view class='img-list'>
      <block wx:for="{{fileBase64}}" wx:key="">
        <view class='img-item'>
          <image src='data:image/jpg;base64,{{item}}' data-index='{{index}}' class='img' mode='aspectFill'></image>
          <view class="upload_progress" style='opacity: {{0.7*(1-item.upload_percent/100)}}' wx:if="{{item.upload_percent < 100}}">{{item.upload_percent}}%</view>
          <view class='delete' data-index='{{index}}' catchtap='deleteImg'></view>
        </view>
      </block>
      <view class='chooseimg' bindtap='uploadDetailImage' wx:if="{{ uploadMax === -1 || uploadMax > tempFiles.length }}">
        <view class="weui-uploader__input-box"></view>
      </view>
    </view>
  </view>
</view>
<canvas canvas-id="canvas0" style="width:{{cWidth0}}px;height:{{cHeight0}}px;position: absolute;left:-1000px;top:-1000px;"></canvas>
<canvas canvas-id="canvas1" style="width:{{cWidth1}}px;height:{{cHeight1}}px;position: absolute;left:-1000px;top:-1000px;"></canvas>
<canvas canvas-id="canvas2" style="width:{{cWidth2}}px;height:{{cHeight2}}px;position: absolute;left:-1000px;top:-1000px;"></canvas>
<canvas canvas-id="canvas3" style="width:{{cWidth3}}px;height:{{cHeight3}}px;position: absolute;left:-1000px;top:-1000px;"></canvas>
<canvas canvas-id="canvas4" style="width:{{cWidth4}}px;height:{{cHeight4}}px;position: absolute;left:-1000px;top:-1000px;"></canvas>
```

CCS篇幅较长，在最后面
### 注意事项/有坑未填：
* 上传图片重复没有过滤
* 最多只能上传5张图片
* 图片太大、绘制太长，会出现空白


## js部分
``` javascript
// 选取图片的方法
  uploadDetailImage: function (e) {
    let that = this;
    const success = (res) => {
      // 返回原图临时路径,res.tempFilePaths
      res.tempFilePaths.forEach((item, index) => {
        // 获取图片的信息
        wx.getImageInfo({
          src: item,
          success: function (res) {
            //----------绘制图形并取出图片路径--------------
            var ratio = 2;
            var canvasWidth = res.width //图片原始长宽
            var canvasHeight = res.height
            while (canvasWidth > 600 || canvasHeight > 600) {// 保证宽高在400以内
              canvasWidth = Math.trunc(res.width / ratio)
              canvasHeight = Math.trunc(res.height / ratio)
              ratio++;
            }
            that.setData({
              ['cWidth'+index]: canvasWidth,
              ['cHeight' + index]: canvasHeight
            })
            // 创建canvas
            var ctx = wx.createCanvasContext('canvas' + index)
            ctx.drawImage(res.path, 0, 0, canvasWidth, canvasHeight)
            ctx.draw(false, setTimeout(function () {
              // 把当前画布指定区域的内容导出生成指定大小的图片
              wx.canvasToTempFilePath({
                canvasId: 'canvas' + index,
                fileType: 'jpg',
                success: function (res) {
                  // 生成base64
                  wx.getFileSystemManager().readFile({
                    filePath: res.tempFilePath,
                    encoding: 'base64',
                    success: res => {
                      var fileBase64 = that.data.fileBase64
                      fileBase64.push(res.data);
                      that.setData({
                        fileBase64: fileBase64
                      })
                    }
                  })
                },
                fail: function (res) {
                  console.log(res.errMsg)
                }
              })
            }, 500))  // 留给绘制时间
          }
        })
      })
    }

    // 调用微信选择图片api
    wx.chooseImage({
      count,
      sizeType,
      sourceType,
      success
    })
  },
  // 删除图片
  deleteImg(e) {
    let index = e.currentTarget.dataset.index
    let arr = this.data.fileBase64
    arr.splice(index, 1)
    this.setData({
      fileBase64: arr
    })
  }
```



## CSS内容
``` css
.content {
  width: 100%;
  background-color: #fff;
}

.content .img-list::after {
  content: '';
  display: block;
  clear: both;
  visibility: hidden;
  height: 0;
}

.content .img-item {
  float: left;
  width: 160rpx;
  height: 160rpx;
  margin: 0 18rpx 18rpx 0;
  border-radius: 8rpx;
  position: relative;
  /* padding: 8rpx; */
  /* border: 1rpx solid #f5222d; */
  /* box-sizing: border-box; */
}

.content .img-item .delete {
  width: 30rpx;
  height: 30rpx;
  position: absolute;
  right: -10rpx;
  top: -10rpx;
  text-align: right;
  vertical-align: top;
  z-index: 2;
  background-size: 30rpx auto;
  background-color: #fff;
  border: 4rpx solid #fff;
  border-radius: 50%;
  background-image: url("data:image/svg+xml;charset=utf-8,%3Csvg width='16' height='16' viewBox='0 0 16 16' xmlns='http://www.w3.org/2000/svg'%3E%3Cg fill='none' fill-rule='evenodd'%3E%3Ccircle fill-opacity='.4' fill='%23404040' cx='8' cy='8' r='8'/%3E%3Cpath d='M11.898 4.101a.345.345 0 0 0-.488 0L8 7.511l-3.411-3.41a.345.345 0 0 0-.488.488l3.411 3.41-3.41 3.412a.345.345 0 0 0 .488.488L8 8.487l3.411 3.411a.345.345 0 0 0 .488-.488L8.488 8l3.41-3.412a.344.344 0 0 0 0-.487z' fill='%23FFF'/%3E%3C/g%3E%3C/svg%3E");
}

.content .img-item .img {
  display: block;
  width: 100%;
  height: 100%;
  border-radius: 8rpx;
}

/* 上传进度 */
.upload_progress {
  position: absolute;
  top: 0;
  right: 0;
  /* opacity: 0.7; */
  border-radius: 8rpx;
  background-color: #000;
  color: #fff;
  width: 158rpx;
  height: 158rpx;
  text-align: center;
  line-height: 158rpx;
  font-size: 24rpx;
}

.chooseimg {
  background-color: #fff;
}

.weui-uploader__input-box {
  float: left;
  position: relative;
  margin-right: 9px;
  margin-bottom: 9px;
  width: 160rpx;
  height: 160rpx;
  border: 1px dashed #ebebeb;
  border-radius: 8rpx;
}

.weui-uploader__input-box:before {
  width: 2px;
  height: 39px;
}

.weui-uploader__input-box:after {
  width: 39px;
  height: 2px;
}

.weui-uploader__input-box:after, .weui-uploader__input-box:before {
  content: " ";
  position: absolute;
  top: 50%;
  left: 50%;
  -webkit-transform: translate(-50%, -50%);
  transform: translate(-50%, -50%);
  background-color: #d9d9d9;
}

.tips {
  color: #666;
  font-size: 24rpx;
  padding-bottom: 20rpx;
}

.img-box {
  width: 100%;
  padding: 20rpx 10rpx 0 28rpx;
}
```