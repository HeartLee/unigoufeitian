<template>
	
	<view>	   
	<view class="btnView" @click="sureClick" >退出登录</view>
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
		  sureClick: function (){
			  localStorage.removeItem("token");
			  localStorage.removeItem("opendid");
			  uni.navigateTo({
			      url: '../login/login' 
			  })
		    },
		},
	}
</script>

<style>
	page {
	  background-color: rgb(245, 245, 245);
	}
	 
	.buttonView{
	  display: flex;
	  height: 100rpx;
	  justify-content: center;
	  align-items: center;
	  /* width: 200rpx; */
	  position: absolute; 
	  right: 0; /* 靠右调节 */
	  margin-right: 60rpx;
	  color: #f65535;
	}
	button::after {
	  border: none;
	 
	 }
	.buttonView button{
	  padding: 0rpx;
	  background-color: #fff;
	  color: #f65535;
	  font-size: 28rpx;
	}
	.btnView{
	  position: absolute; 
	  top: 120rpx;
	  left: 30rpx;
	  right: 30rpx;
	  height: 100rpx;
	  background-color: #f65535;
	  color: white;
	  border-radius: 50rpx;
	  display: flex;
	  justify-content: center;
	  align-items: center;
	}
</style>
