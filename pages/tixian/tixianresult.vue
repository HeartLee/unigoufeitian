<template>
	<!--pages/tixian/tixianresult.wxml-->
	<!--pages/operation/operation.wxml-->
	<view>
	<view class="contentView">
	  <image src="../../static/iconsure.png" mode="widthFix"></image>
	</view>
	<view class="titleView">提现成功</view>
	<view class="subTitleView">支付宝到账预计2个工作日,请注意查收！</view>
	<view class="btn2" @click="backToHome">返回首页</view>
	</view>
</template>

<script>
	
	export default {
		data() {
		return {
		}
		},
		onLoad(option) {
	
			
		},
		methods:{
		  backToHome(){
		    uni.switchTab({
				url:'../index/index'
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
	  margin-top: 50rpx;
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
