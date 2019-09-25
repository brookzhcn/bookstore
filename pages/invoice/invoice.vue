<template>
	<view class="page">
	  <form bindsubmit="bindSave" report-submit="true">
	  <view class="page__bd">    
	    <view class="weui-cells__title">开票信息</view>
	    <view class="weui-cells weui-cells_after-title">
	      <view class="weui-cell weui-cell_input">
	        <view class="weui-cell__hd">
	          <view class="weui-label">抬头</view>
	        </view>
	        <view class="weui-cell__bd">
	          <input class="weui-input price" placeholder='请填写公司名称' name='comName'/>
	        </view>
	      </view>
	      <view class="weui-cell weui-cell_input">
	        <view class="weui-cell__hd">
	          <view class="weui-label">税号</view>
	        </view>
	        <view class="weui-cell__bd">
	          <input class="weui-input price" placeholder='请填写开票税号' name='tfn'/>
	        </view>
	      </view>
	      <view class="weui-cell weui-cell_input">
	        <view class="weui-cell__hd">
	          <view class="weui-label">发票内容</view>
	        </view>
	        <view class="weui-cell__bd">
	          <input class="weui-input price" name='consumption' value='商品明细' />
	        </view>
	      </view>
	      <view class="weui-cell weui-cell_input">
	        <view class="weui-cell__hd">
	          <view class="weui-label">联系方式</view>
	        </view>
	        <view class="weui-cell__bd">
	          <input class="weui-input price" name='mobile' placeholder='注册的手机号码' />
	        </view>
	      </view>
	      <view class="weui-cell weui-cell_input">
	        <view class="weui-cell__hd">
	          <view class="weui-label">开票金额</view>
	        </view>
	        <view class="weui-cell__bd">
	          <input class="weui-input price" name='amount' placeholder='0.00' />
	        </view>
	      </view>
	    </view>
	
	    <view class="weui-cells__title">快递地址</view>
	    <view class="weui-cells weui-cells_after-title">
	      <view class="weui-cell">
	        <view class="weui-cell__bd">
	          <textarea name='remark' class="weui-textarea" placeholder="请填写快递地址" style="height: 5em" />
	        </view>
	      </view>
	    </view>
	
	    <view class="weui-btn-area">      
	      <button class="weui-btn" type="warn" formType="submit">申请开票</button>
	    </view>
	  </view>
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
			
		  async bindSave(e) {
			// 提交保存
			WXAPI.addTempleMsgFormid({
			  token: wx.getStorageSync('token'),
			  type: 'form',
			  formId: e.detail.formId
			})
			const _this = this;
			let comName = e.detail.value.comName;
			let tfn = e.detail.value.tfn;
			let mobile = e.detail.value.mobile;
			let amount = e.detail.value.amount;
			let consumption = e.detail.value.consumption;
			let remark = e.detail.value.remark;
			if (!comName) {
			  wx.showToast({
				title: '公司名称不能为空',
				icon: 'none'
			  })
			  return
			}
			if (!tfn) {
			  wx.showToast({
				title: '税号不能为空',
				icon: 'none'
			  })
			  return
			}
			if (!consumption) {
			  wx.showToast({
				title: '发票内容不能为空',
				icon: 'none'
			  })
			  return
			}
			if (!mobile) {
			  wx.showToast({
				title: '请填写您的手机号码',
				icon: 'none'
			  })
			  return
			}
			if (!remark) {
			  wx.showToast({
				title: '快递地址不能为空',
				icon: 'none'
			  })
			  return
			}
			if (!amount || amount*1 < 100) {
			  wx.showToast({
				title: '开票金额不能低于100',
				icon: 'none'
			  })
			  return
			}
			const extJsonStr = {}
			extJsonStr['手机号码'] = mobile
			WXAPI.invoiceApply({
			  token: wx.getStorageSync('token'),
			  comName,
			  tfn,
			  amount,
			  consumption,
			  remark,
			  extJsonStr: JSON.stringify(extJsonStr)
			}).then(res => {
			  if (res.code == 0) {
				wx.showModal({
				  title: '成功',
				  content: '提交成功，请耐心等待我们处理！',
				  showCancel: false,
				  confirmText: '我知道了',
				  success(res) {
					wx.navigateTo({
					  url: "/pages/invoice/list"
					})
				  }
				})
			  } else {
				wx.showModal({
				  title: '失败',
				  content: res.msg,
				  showCancel: false,
				  confirmText: '我知道了'
				})
			  }
			})
		  }
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
.weui-cell:first-child:before {
  display: none;
}
.weui-cell_active {
  background-color: #ECECEC;
}
.weui-cell_primary {
  -webkit-box-align: start;
  -webkit-align-items: flex-start;
          align-items: flex-start;
}
.weui-cell__hd_in-select-after,
.weui-cell__bd {
  -webkit-box-flex: 1;
  -webkit-flex: 1;
          flex: 1;
}
.weui-cell__ft_in-radio {
  padding-left: 0.35em;
}
.weui-cell_input {
  padding-top: 0;
  padding-bottom: 0;
}
.weui-label {
  width: 105px;
  word-wrap: break-word;
  word-break: break-all;
}
.weui-input {
  height: 2.58823529em;
  min-height: 2.58823529em;
  line-height: 2.58823529em;
}
</style>
