<template>
<view class="container">
  <view class="main">
	<view class="qrcode item">
	  <canvas style="width: 200px; height: 200px;" canvas-id="myQrcode"></canvas>
	  <view class="tips">{{text}}</view>
	</view>
	<view class="input-container item">
	  <input bindinput="bindKeyInput" placeholder="输入转换内容"/>
	  <button type="default"  bindtap="changeText">提交</button>
	</view>
	<view class="item">
	  <button type="default"  bindtap="download">下载</button>
	  <button type="default"  bindtap="repaint">设置x,y</button>
	  <button type="default"  bindtap="getBase64Data">base64</button>
	</view>
	<view class="round left"></view>
	<view class="round right"></view>
	<view class="intro item">
	  <view class="title">说明：</view>
	  <view>1. 可自定义canvas宽高。</view>
	  <view>2. 支持修改二维码的计算模式、纠错级别、背景色、前景色。</view>
	  <view>3. 更多详细说明，请参考项目README。</view>
	</view>
  </view>
</view>
</template>

<script>
import drawQrcode from '../../utils/weapp.qrcode.js'

Page({
  data: {
    text: 'https://m.baidu.com',
    inputValue: ''
  },
  onLoad () {
    this.draw()
  },
  changeText (text) {
    if (!this.data.inputValue) {
      wx.showModal({
        title: '提示',
        content: '请先输入要转换的内容！',
        showCancel: false
      })
      return
    }
    this.setData({
      text: this.data.inputValue
    })
    this.draw()
  },
  bindKeyInput (e) {
    this.setData({
      inputValue: e.detail.value
    })
  },
  draw () {
    drawQrcode({
      width: 160,
      height: 160,
      x: 20,
      y: 20,
      canvasId: 'myQrcode',
      // ctx: wx.createCanvasContext('myQrcode'),
      typeNumber: 10,
      text: this.data.text,
      image: {
        imageResource: '../../images/icon.png',
        dx: 70,
        dy: 70,
        dWidth: 60,
        dHeight: 60
      },
      callback(e) {
        console.log('e: ', e)
      }
    })
  },
  repaint () {
    // 设置二维码起始位置 x,y
    drawQrcode({
      width: 160,
      height: 160,
      x: 20,
      y: 20,
      canvasId: 'myQrcode',
      typeNumber: 10,
      text: this.data.text,
      callback(e) {
        console.log('e: ', e)
      }
    })
  },
  download () {
    // 导出图片
    wx.canvasToTempFilePath({
      x: 0,
      y: 0,
      width: 300,
      height: 300,
      destWidth: 300,
      destHeight: 300,
      canvasId: 'myQrcode',
      success(res) {
        console.log('图片的临时路径为：', res.tempFilePath)
        let tempFilePath = res.tempFilePath
        // 保存图片，获取地址
        // wx.saveFile({
        //   tempFilePath,
        //   success (res) {
        //     const savedFilePath = res.savedFilePath
        //     console.log('savedFilePath', savedFilePath)
        //   }
        // })

        // 保存到相册
        wx.saveImageToPhotosAlbum({
          filePath: tempFilePath,
          success: function (res) {
            wx.showToast({
              title: '保存成功',
              icon: 'success',
              duration: 2000
            })
          },
          fail: function (res) {
            wx.showToast({
              title: '保存失败',
              icon: 'none',
              duration: 2000
            })
          }
        })
      }
    })
  },
  getBase64Data () {
    // 获取二维码 base64 格式
    wx.canvasToTempFilePath({
      x: 0,
      y: 0,
      width: 300,
      height: 300,
      destWidth: 300,
      destHeight: 300,
      canvasId: 'myQrcode',
      success(res) {
        console.log('图片的临时路径为：', res.tempFilePath)
        let tempFilePath = res.tempFilePath
        // 获取 base64
        wx.getFileSystemManager().readFile({
          filePath: tempFilePath,
          encoding: 'base64',
          success: function (res) {
            console.log('[base64 data is]', res)
            wx.showToast({
              title: '获取成功',
              icon: 'success',
              duration: 2000
            })
          },
          fail: function (res) {
            wx.showToast({
              title: '获取失败',
              icon: 'none',
              duration: 2000
            })
          }
        })
      }
    })
  }
})
</script>

<style>
Page {
  background-color: #383c42;
}
.main {
  width: 670rpx;
  margin: 40rpx auto 0;
  background: #fff;
  box-shadow: 0 0 2rpx 0 rgba(25, 29, 33, 0.15);
  border-radius: 12rpx;
  padding: 0 40rpx;
  box-sizing: border-box;
  position: relative;
}
.item {
  border-bottom: 2rpx solid #F4F5F7;
}
.item > button {
  height: 60rpx;
  font-size: 30rpx;
  line-height: 60rpx;
  display: inline-block;
  padding: 0 15rpx;
  margin: 10rpx 20rpx 0 0;
}
.qrcode {
  width: 590rpx;
  margin: 0 auto;
  padding: 50rpx 0 59rpx;
  border-top: 2rpx solid #F4F5F7;
}
.qrcode > view, .qrcode > text {
  text-align: center;
}
canvas {
  margin: 58rpx auto 0;
}
.type {
  margin-top: 24rpx;
  font-size: 28rpx;
  color: #585C64;
  line-height: 32rpx;
}
.tips {
  margin-top: 40rpx;
  font-size: 24rpx;
  color: #999BA1;
  line-height: 28rpx;
}
.input-container {
  padding: 10rpx 0 0 0;
  width: 590rpx;
  margin: 0 auto;
  height: 102rpx;
}
.input-container > input {
  display: inline-block;
  height: 80rpx;
  font-size: 28rpx;
  color: #191D21;
  line-height: 102rpx;
  width: 450rpx;
}
.input-container > button {
  width: 120rpx;
  height: 60rpx;
  font-size: 30rpx;
  line-height: 60rpx;
  display: inline-block;
  float: right;
  margin-top: 10rpx;
}
.arrow {
  width: 28rpx;
  height: 28rpx;
  vertical-align: top;
  margin-top: 36rpx;
  float: right;
}
.round {
  width: 24rpx;
  height: 24rpx;
  border-radius: 50%;
  background-color: #383c42;
  position: absolute;
  top: 735rpx;
}
.left {
  left: -12rpx;
}
.right {
  right: -12rpx;
}
.intro {
  width: 590rpx;
  margin-top: 20rpx;
}
.intro > view{
  color: #999BA1;
  font-size: 28rpx;
  line-height: 48rpx;
  word-wrap: break-word;
  margin-bottom: 8rpx;
}
.intro > .title {
  font-size: 32rpx;
  color: #191D21;
  line-height: 50rpx;
}
</style>
