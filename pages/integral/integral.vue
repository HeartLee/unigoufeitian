<template>
	<view>
		<view class="headerView">
		  <view class="intrgralView">
		    <view class="intrgralViewTitle">可用积分</view>
		    <image src="../../static/jifen-2.png" style="width:50rpx;height:50rpx;margin-left: 20rpx"></image>
		    <view class="intrgralViewvaule">{{userInfo.money}}</view>
		  </view>
		  <view class="infoView">注：1积分可兑换1元现金</view>
		    <view class="btnBgView">
		      <view class="status">
		        <button @click="tixiandianji">提现</button>
		      </view>
		    </view>
		</view>
		<view class="tipLabelView">
		  <view class="tipLabel">余额明细</view>
		  <view class="tipinfoLabel">由于访问量大，余额明细将在下午14:00后展示</view>
		</view>
		<view class="boxheaderView">
		    <view class="boxtitle">类型</view>
		     <view class="boxtitle">积分</view>
		      <view class="boxtitle" style="width: 46%;">说明</view>
		</view>
		<view style="margin-top: 120rpx;">
		  <block v-for="(item,index) in dataList" :key="index">  
		      <view class="boxList" >
		      <view class="boxListTile" v-if="item.type=='1'"> 收入</view>
		      <view class="boxListTile" v-else> 提现</view>
		      <view class="boxListTile">{{item.money}}</view>
		        <view class="boxListTile" style="width: 46%;">{{item.content}}</view>
		  </view>
		  </block>
		 </view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				 userInfo:{},
				dataList:[],
			}
		},
		onLoad(option) {
		
			   this.getPersoninfo();
			 this.getMoneyRecord();
		},
		methods:{
			 tixiandianji:function () {
			    uni.navigateTo({
			      url: '../tixian/tixian?money='+this.userInfo.money
			    })
			  },
			  getPersoninfo: function (options) {
			      var that = this;
			      uni.request({
			          url: 'https://srxadmin.goufeitian.com/index.php/Api/Index/getPersoninfo',
			          data: {
			             openid: localStorage.getItem("opendid")
			          },
			          success: function (res) {
			              if (res.data.status == '1') {
							  that.userInfo = res.data.data
			              } else {
			                  uni.showToast({
			                      title: res.data.info,
			                      icon: 'none',
			                      duration: 2000
			                  })
			              }
			          },
			      })
			  },  
			    getMoneyRecord:function (options) {
			      var that = this;
			      
			      uni.request({
			          url: 'https://srxadmin.goufeitian.com/index.php/Api/Index/getMoneyRecord',
			          data: {
			            openid: localStorage.getItem("opendid"),
			              type:'',
			              keywords:'',
			              page:'1',
			          },
			          success: function (res) {
			           
			              if (res.data.status == '1') {
							that.dataList = res.data.data
			             
			              } else {
			                  uni.showToast({
			                      title: res.data.info,
			                      icon: 'none',
			                      duration: 2000
			                  })
			              }
			          },
			      })
			    },
		},
	}
</script>

<style>
	/* pages/integral/integral.wxss */
	page {
	  background-color: rgb(245, 245, 245);
	}
	.headerView{
	  background-color: #f65535;
	  padding-bottom: 30rpx;
	  margin-left: 0rpx;
	  width: 100%;
	  padding-top: 80rpx;
	}
	.intrgralView{
	  align-items: center;
	  display: flex;  
	  flex-direction: row; 
	  height: 50rpx;
	}
	.intrgralViewTitle{
	  width: 53%;
	  
	  text-align: right;
	  color: white;
	  font-size: 28rpx;
	  line-height: 50rpx;
	}
	.intrgralViewvaule{
	  margin-left: 10rpx;
	  color: white;
	  font-size: 36rpx;
	  line-height: 50rpx;
	}
	.infoView{
	  width: 100%;
	  font-size: 28rpx;
	  margin-top: 20rpx;
	  color: white;
	  text-align: center;
	}
	.btnBgView{
	  margin-top: 40rpx;
	  width: 100%;
	  height:  80rpx;
	  display: flex;
	  flex-direction: column;
	  align-items: center;
	}
	.status button {
	  text-align: center;
	  border-radius: 40rpx;
	  font-size: 32rpx;
	  margin-left: 20rpx;
	  width: 240rpx;
	  height: 80rpx;
	  color: #f65535;
	  line-height: 80rpx;
	}
	.tipLabelView{
	  margin-top: 20rpx;
	  width: 100%;
	  display: flex;  
	  flex-direction: row; 
	  align-items: center;
	}
	.tipLabel{
	  margin-left: 30rpx;
	  font-size: 28rpx;
	  color: #333;
	}
	.tipinfoLabel{
	  margin-left: 10rpx;
	  font-size: 24rpx;
	  color: #999;
	}
	.boxheaderView{
	  margin-top: 50rpx;
	  margin-left: 30rpx;
	  width: calc(100% - 60rpx);
	  position: absolute;
	  background-color:rgb(252, 249, 240) ;
	  /* border-radius: 35rpx; */
	  display: flex;  
	  flex-direction: row; 
	  align-items: center;
	  padding: 2%;
	}
	.boxtitle{
	  font-size: 24;
	  color: #333;
	  width: 23%;
	  margin-left: 2%;
	  text-align: center;
	}
	.boxList{
	  margin-left: 30rpx;
	  width: calc(100% - 60rpx);
	  display: flex;  
	  flex-direction: row; 
	  background-color: #fff;
	  align-items: center;
	  padding: 2%;
	  border-bottom: 1rpx solid rgba(0, 0, 0, 0.1);
	}
	.boxList .boxListTile{
	  font-size: 24;
	  color: #333;
	  width: 23%;
	  margin-left: 2%;
	  text-align: center;
	}
</style>
