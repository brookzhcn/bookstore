<template>
	<view class="container">
	    <form bindsubmit="bindSave">
	    <view class="form-box">
	        <view class="row-wrap">
	            <view class="label">充值金额</view>
	            <view class="label-right">
	                <input name="amount" class="input" type="text" placeholder="0.00"/>
	            </view>
	        </view>
	    </view>
	    <button type="warn" class="save-btn" formType="submit">立即支付</button>
	    </form>
	</view>
</template>

<script>
	var wxpay = require('../../utils/pay.js')
	var app = getApp()
	Page({
	
	  data: {
	    uid: undefined,
	  },
	

	  onLoad: function (options) {
	    let recharge_amount_min = app.globalData.recharge_amount_min;
	    if (!recharge_amount_min) {
	      recharge_amount_min = 0;
	    }
	    this.setData({
	      uid: wx.getStorageSync('uid'),
	      recharge_amount_min: recharge_amount_min
	    });
	  },
	  
	  onShareAppMessage: function () {
	  
	  },
	  bindCancel: function () {
	    wx.navigateBack({})
	  },
	  bindSave: function (e) {
	    var that = this;
	    var amount = e.detail.value.amount;
	
	    if (amount == "" || amount * 1 < 0) {
	      wx.showModal({
	        title: '错误',
	        content: '请填写正确的充值金额',
	        showCancel: false
	      })
	      return
	    }
	    if (amount * 1 < that.data.recharge_amount_min * 1) {
	      wx.showModal({
	        title: '错误',
	        content: '单次充值金额至少' + that.data.recharge_amount_min + '元',
	        showCancel: false
	      })
	      return
	    }
	    wxpay.wxpay(app, amount, 0, "/pages/ucenter/index");
	  }
	})
</script>

<style>
	page{
    height: 100%;
}
.container{
    background-color: #f5f5f9;
    justify-content: initial;
}
.form-box{
    width:100%;
    background-color: #fff;
    margin-top: 20rpx;
}
.row-wrap{
    width: 720rpx;
    height: 88rpx;
    line-height: 88rpx;
    margin-left: 30rpx;
    border-bottom: 1rpx solid #f4f4f4;
    display: flex;
    font-size: 28rpx;
    /*justify-content: space-between;*/
}
.row-wrap .label{
    width: 160rpx;
    color: #000
}
.row-wrap .label-right{
    flex: 1;
    height: 88rpx;
    line-height: 88rpx;
}
.row-wrap .label-right input{
    height: 100%;
    font-size: 28rpx;
    padding-right: 30rpx;
}
.row-wrap .right-box{
    margin-right: 30rpx; 
}
.arrow-right{
    width: 15rpx;
    height: 24rpx;
}
.save-btn,
.cancel-btn{
    width: 690rpx;
    height: 80rpx;
    line-height: 80rpx;
    text-align: center;
    margin-top:30rpx; 
    border-radius: 6rpx;
    box-sizing: border-box;
}
.save-btn{
    background-color: #e64340;
    color:#fff;
}
button[type="default"]{
    background-color: #ffffff;
    color:#000;
}
.addr-details{
    height: auto;
    padding: 30rpx 0;
    margin-left:30rpx;
    border-bottom: 1rpx solid #f4f4f4;
    display: flex;
    font-size: 28rpx;
}
.addr-details .label{
    margin:auto 0 auto 0;
    width: 160rpx;
    color: #000
}
.addr-details textarea{
    box-sizing: border-box;
    width: 480rpx;
    overflow: scroll;
}
picker {
    min-width: 20rpx;
    height: 100%;
    margin-right: 20rpx;
}
.hui{
    color: #777;
}
</style>
