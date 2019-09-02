<!-- <template>
	<view class="container">
	<button class='my-sign'  v-if="score_sign_continuous == 0" size="small" type="primary" @click="scoresign">每日签到</button>
	<button class='my-sign' v-else-if="score_sign_continuous > 0" size="small" type="warn" @click="scoresign">已连续签到 {{score_sign_continuous}} 天<button>
	</view>
</template>

<script>
	const app = getApp()
	
export default {
	data() {
		return {
			score_sign_continuous: 0
		}
	},
	methods: {
		 scoresign: function () {
			var that = this;
			wx.request({
			  url: 'https://api.it120.cc/' + app.globalData.subDomain + '/score/sign',
			  data: {
				token: wx.getStorageSync('token')
			  },
			  success: function (res) {
				if (res.code == 0) {
				  that.getUserAmount();
				  that.checkScoreSign();
				} else {
				  wx.showModal({
					title: '错误',
					content: res.msg,
					showCancel: false
				  })
				}
			  }
			})
		  },
		  checkScoreSign: function () {
			var that = this;
			wx.request({
			  url: 'https://api.it120.cc/' + app.globalData.subDomain + '/score/today-signed',
			  data: {
				token: wx.getStorageSync('token')
			  },
			  success: function (res) {
				if (res.code == 0) {
				  that.setData({
					score_sign_continuous: res.continuous
				  });
				}
			  }
			})
		  },  
		getUserAmount: function () {
		    var that = this;
		    wx.request({
		      url: 'https://api.it120.cc/' + app.globalData.subDomain + '/user/amount',
		      data: {
		        token: wx.getStorageSync('token')
		      },
		      success: function (res) {
		        if (res.data.code == 0) {
		          that.setData({
		            balance: res.balance,
		            freeze: res.freeze,
		            score: res.score
		          });
		        }
		      }
		    })
		
		  },  
		
},
}
	
</script>

<style>
.my-sign{
  position: absolute;
  top: 10rpx;
  right: 30rpx;
  z-index: 100;
  margin-top: 20rpx;
  font-size: 24rpx;
}
</style>
 -->