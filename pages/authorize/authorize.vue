<template>
	<view class="container">
		<form bindsubmit="bindSave">
		<image class="logo" src="/static/wx.png" mode="widthFix" />
		<view class="title">绑定手机号码</view>
		<view class="profile">授权并同意读取当前微信的手机号码</view>
		<button type="primary" open-type="getPhoneNumber" @click="getPhoneNumber" class="weui-btn mini-btn">立即绑定</button>
		</form>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			};
		},
		methods:{
			getPhoneNumber(e) {
			  if (!e.detail.errMsg || e.detail.errMsg != "getPhoneNumber:ok") {
			    wx.showModal({
			      title: '错误',
			      content: e.detail.errMsg,
			      showCancel: false
			    })
			    wx.reLaunch({
			      url: "/pages/index/index"
			    })
			    return;
			  }
			  var that = this;
			  WXAPI.bindMobile({
			    token: wx.getStorageSync('token'),
			    encryptedData: e.detail.encryptedData,
			    iv: e.detail.iv
			  }).then(function (res) {
			    if (res.code == 0) {
			      wx.showToast({
			        title: '绑定成功',
			        icon: 'success',
			        duration: 2000
			      })
			    } else {
			      wx.showModal({
			        title: '提示',
			        content: '绑定失败',
			        showCancel: false
			      })
			    }
			    wx.reLaunch({
			      url: "/pages/index/index"
			    })
			  })
			},
		}
	}
</script>

<style>    
	form {
	  width:750rpx;
	}
	.logo {
	  width:200rpx;
	  margin-top:180rpx;
	  margin-left:275rpx;
	}
	.title {
	  text-align: center;
	  margin-top:70rpx;
	}
	.profile {
	  text-align: center;
	  margin-top:30rpx;
	  font-size: 14px;
	  color:#888;
	}
	.mini-btn{
	  margin-top: 50rpx;
	  margin-left: 50px;
	  margin-right: 50px;
	}
</style>
