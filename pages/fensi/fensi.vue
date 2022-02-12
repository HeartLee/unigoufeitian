<template>
	
	<view class="orderInfo">
	  <view class="orderInfo-item" v-for="(item,index) in sendList" :key="index"> 
	    <image :src="item.avaimg" style="width:100rpx;height:100rpx;"></image>
	    <span class="coninfo">{{item.nickname}}</span>
	    
	    <span class="timeinfo" style="left:120rpx">获得积分：{{item.money}}</span>
	 
	   </view> 
	</view>
</template>

<script>
	
	export default {
		data() {
			return {
				    sendList: [],
				
			}
		},
		onLoad(option) {
		this.getData()
			
		},
		methods:{
			 getData: function () {
			    var _this = this;
			    wx.request({
			      url: 'https://srxadmin.goufeitian.com/index.php/Api/Index/getFensi',
			      data: {
			        type: 0,
			        openid: localStorage.getItem("opendid")
			      },
			      success:function (res) {
			        var content = res.data.data;
					_this.sendList = content;
			      }
			    })
			  
			  },
		},
	}
</script>

<style>
	.tabNav{z-index: 4; position: fixed; top:0;left:0; width:100%; height:80rpx; line-height: 80rpx; background: #fff; display: flex; border-bottom:1px solid #f5f5f5; box-sizing: border-box; }
	.tabNav> view{text-align: center; margin-right:50rpx;}
	.tabNav> view:last-child{ margin-right: 0;}
	.tabNav> view text{padding:0 15rpx; height:75rpx; display:inline-block;}
	.tabNav .cur text{ border-bottom:5rpx solid green; color:#000;}
	.orderInfo{padding:0 20px;margin-top:0rpx;background-color: #fff;}
	.orderInfo-item{margin:20px 0;position: relative}
	.orderInfo-item .coninfo{position: absolute;left:120rpx;top:20rpx;}
	.orderInfo-item .timeinfo{position: absolute;right:10rpx;top:70rpx;color:#999999}
	page{
	  background-color: #fff;
	}
</style>
