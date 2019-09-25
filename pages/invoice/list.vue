<template>
	<view v-if="invoiceList.length <= 0" class="weui-loadmore weui-loadmore_line" style='margin-top:200rpx;'>
	  <view class="weui-loadmore__tips weui-loadmore__tips_in-line">暂无数据</view>
	</view>
	<view v-else class="weui-panel weui-panel_access">
	  <view class="weui-panel__bd">
	    <view v-for="item in invoiceList" :key="item.status" class="weui-media-box weui-media-box_text">
	      <view class="weui-media-box__title weui-media-box__title_in-text">
	        <text style='color:red'>￥ {{ item.amount }}</text>
	        <view class="weui-badge" style="margin-left: 5px;">{{item.consumption }}</view>
	        <text v-if="item.status == 0" style='color:gray;margin-left:10px;font-size:14px;'>待处理</text>
	        <text v-if="item.status == 1" style='color:red;margin-left:10px;font-size:14px;'>不通过</text>
	        <text v-if="item.status == 2" style='color:orange;margin-left:10px;font-size:14px;'>开票中</text>
	        <text v-if="item.status == 3" style='color:green;margin-left:10px;font-size:14px;'>已开票</text>
	      </view>
	      <view class="weui-media-box__desc">{{ item.comName }}</view>
	      <view class="weui-media-box__desc" style='margin-top:10rpx;'>{{ item.dateAdd }}</view>
	    </view>
	  </view>
	</view>
</template>

<script>
	// const WXAPI = require('../../wxapi/main')
	// const AUTH = require('../../utils/auth')
	// import WXAPI from '../../wxapi/main.js';
	// import AUTH from '../../utils/auth.js';
	import {
	    mapMutations  
	} from 'vuex';
	
	export default {
		data() {
			return {
				invoiceList: []
			}
		},
		methods: {
			...mapMutations(['logout']),
			
			// onShow: function () {
			//   const that = this
			//   AUTH.checkHasLogined().then(isLogined => {
			//     if (isLogined) {
			//       WXAPI.invoiceList({
			//         token: wx.getStorageSync('token')
			//       }).then(res => {
			//         if (res.code == 0) {
			//             invoiceList: res.result
			//         } else {
			//             invoiceList: []
			//         }
			//       })
			//     } else {
			//       wx.showModal({
			//         title: '提示',
			//         content: '本次操作需要您的登录授权',
			//         cancelText: '暂不登录',
			//         confirmText: '前往登录',
			//         success(res) {
			//           if (res.confirm) {
			//             wx.switchTab({
			//               url: "/pages/user/user"
			//             })
			//           } else {
			//             wx.navigateBack()
			//           }
			//         }
			//       })
			//     }
			//   })
			// },
			
		}
	}
	
</script>

<style>

</style>
