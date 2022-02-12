<template>
<view >
	   <view class="contentView">
	     <image src="../../static/iconsure.png" mode="widthFix"></image>
	   </view>
	   <view class="titleView">提交资料成功</view>
	   <view class="subTitleView">审核会在三个工作日之内完成，审核完成之后积分立即到账。</view>
	   <view class="infoTitleView">禁止使用好评返现卡！</view>
	   <view class="infoTitleView" style="margin-bottom: 30rpx;">否则不兑换奖励！</view>
	   
	   <view class="btn1" @click="orderDetail">查看订单</view>
	   <view class="btn2" @click="backToHome">返回首页</view>
  </view>	   
</template>

<script>
export default {
	data() {
		return {
		  orderid:'',
		}
	},
	onLoad(option) {
		this.orderid = option.orderid;
		
	},
	methods:{
		orderDetail(){
			 uni.redirectTo({
				 url:'../detailpage/detailpage?orderid='+ this.orderid
	
			    })
		},
		backToHome(){
			uni.navigateBack({
				
			})
		}
	},
}
</script>

<style>
	/* pages/operation/operation.wxss */
	
	image{
	  width: 160rpx;
	}
	.contentView{
	  width: 100%;
	  margin-top: 80rpx;
	  display: flex;
	  justify-content: center;
	}
	.titleView{
	  margin-top: 50rpx;
	  display: flex;
	  justify-content: center;
	  width: 100%;
	  font-size: 32rpx;
	  color: #000;
	}
	.subTitleView{
	  margin-top: 50rpx;
	  text-align: center;
	  width: 70%;
	  margin-left: 15%;
	  font-size: 32rpx;
	  color: #999;
	  margin-bottom: 15rpx;
	}
	
	.infoTitleView{
	  margin-top: 0rpx;
	  text-align: center;
	  width: 100%;
	  font-size: 36rpx;
	  color: #f65535;
	}
	.btn1{
	  width: 70%;
	  margin-left: 15%;
	  height: 100rpx;
	  line-height: 100rpx;
	  text-align: center;
	  border-radius: 50rpx;
	  background-color: #f65535;
	  color: white;
	}
	
	.btn2{
	  margin-top: 20rpx;
	  width: 70%;
	  margin-left: 15%;
	  height: 100rpx;
	  line-height: 100rpx;
	  text-align: center;
	  border-radius: 50rpx;
	  /* background-color: white; */
	  color: #f65535;
	  border: 1px solid #fa3534;
	}

</style>
