<template>
	<view class="container">
		<view class="page-body">
			<view class="weui-cells__title">扫码结果</view>
			<view class="weui-cells weui-cells_after-title">
				<view class="weui-cell">
					<view class="weui-cell__bd">{{result}}</view>
				</view>
			</view>
			<view class="btn-area">
				<button type="primary" @click="scanCode">扫一扫</button>
			</view>
		</view>
	</view>
</template>


<script>
	
	export default {
		data() {
			return {
				
			};
		},
		methods:{
			
			scanCode() {
				console.log("scan code..")
				const that = this
				wx.scanCode({
					success(res) {
						that.setData({
							result: res.result
						})
					},
					fail() {}
				})
			},
		}
	}
	
</script>

<style>
	page {
		line-height: 1.6;
		font-family: -apple-system-font, "Helvetica Neue", sans-serif;
	}

	icon {
		vertical-align: middle;
	}

	.weui-cells {
		position: relative;
		margin-top: 1.17647059em;
		background-color: #FFFFFF;
		line-height: 1.41176471;
		font-size: 17px;
	}

	.weui-cells:before {
		content: " ";
		position: absolute;
		left: 0;
		top: 0;
		right: 0;
		height: 1px;
		border-top: 1rpx solid #D9D9D9;
		color: #D9D9D9;
	}

	.weui-cells:after {
		content: " ";
		position: absolute;
		left: 0;
		bottom: 0;
		right: 0;
		height: 1px;
		border-bottom: 1rpx solid #D9D9D9;
		color: #D9D9D9;
	}

	.weui-cells__title {
		margin-top: .77em;
		margin-bottom: .3em;
		padding-left: 15px;
		padding-right: 15px;
		color: #999999;
		font-size: 14px;
	}

	.weui-cells_after-title {
		margin-top: 0;
	}

	.weui-cells__tips {
		margin-top: .3em;
		color: #999999;
		padding-left: 15px;
		padding-right: 15px;
		font-size: 14px;
	}

	.weui-cell {
		padding: 10px 15px;
		position: relative;
		display: -webkit-box;
		display: -webkit-flex;
		display: flex;
		-webkit-box-align: center;
		-webkit-align-items: center;
		align-items: center;
	}

	.weui-cell:before {
		content: " ";
		position: absolute;
		left: 0;
		top: 0;
		right: 0;
		height: 1px;
		border-top: 1rpx solid #D9D9D9;
		color: #D9D9D9;
		left: 15px;
	}

	.weui-cell__bd {
		-webkit-box-flex: 1;
		-webkit-flex: 1;
		flex: 1;
	}

	.weui-btn {
		margin-top: 15px;
	}

	.weui-btn-area {
		margin: 1.17647059em 15px 0.3em;
	}
</style>
