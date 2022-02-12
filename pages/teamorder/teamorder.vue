<template>
	
	<view class="orderInfo">
	  <view class="orderInfo-item"  v-for="(item,index) in sendList" :key="index"> 
	    <image :src="item.avaimg" style="width:100rpx;height:100rpx;"></image>
	    <span class="coninfo">{{item.nickname}}</span>
	    <span class="timeinfo">{{item.createtime}}</span>
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
			      url: 'https://srxadmin.goufeitian.com/index.php/Api/Index/teamorder',
			      data: {
			        openid:localStorage.getItem("opendid"),
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
	.orderInfo{
	  padding:0 20px;
	  margin-top:0rpx;
	}
	.orderInfo-item{
	  margin:20px 0;
	  position: relative
	}
	.orderInfo-item .coninfo{
	  position: absolute;
	  left:120rpx;
	  top:25rpx;}
	.orderInfo-item .timeinfo{
	  position: absolute;
	  right:10rpx;
	  top:25rpx;
	  color:#999999
	}
	.orderInfo-item .money{
	  position: absolute;
	  margin-left: 20rpx;
	  top:25rpx;
	  color:#999999
	}
	page{
	  background-color: #fff;
	}

</style>
