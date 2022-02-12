<template>
<view class="main">
	    <view class="header" style="height: 1200px;">
	      <image  src="../../static/yueka.png" style="width:100%; " mode="widthFix" ></image>
	      <image :src="imgurl" style="width:100%; " mode="widthFix"></image>
	
	      <view class="vipinfo" @click="buyVip">
	      <span>购买</span>
	    </view>
	    </view>
  </view>	   
</template>

<script>
export default {
	data() {
		return {
			imgurl:'https://srxadmin.goufeitian.com/Public/uploads/image/20210615/vip_buy.png'
			
		}
	},
	onLoad(option) {
		this.isflog = option.isflog;
		console.log(this.isflog);
		
	},
	methods:{
    buyVip: function () {
      var _this = this;
      wx.request({
        url: 'https://srxadmin.goufeitian.com/index.php/Api/Index/vipBuy',
        data: {
          openid: localStorage.getItem("opendid")
        },
        success:function (res) {

          uni.showToast({
            icon:"none",
            title: res.data.info,
            duration: 2000
          });
        }
      })
    }
	},
}
</script>

<style>
	page {
	  background-color: #ffffff;
	}
	.main{background:#ffffff;min-height:100%;}
	.header{padding:20rpx;margin-top:20rpx;}
	.header img{
	  display: block;
	}
	.vipinfo{
	  display: inline-block;
    margin-top: 50px;
	  margin-left:5%;
	  width: 90%;
	  text-align: center;
	  height: 100rpx;
	  background:#f65535;
	  border-radius: 20rpx;
	  color:#ffffff;
	  line-height:100rpx;
	}
</style>
