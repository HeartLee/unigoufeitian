<template>
<view style="width: 100%;">
	<swiper class="swiper"
	   			indicator-dots="true" 
	   			autoplay="true" 
	   			interval="5000" 
	   			duration="1500"	>
	   			<swiper-item v-for="(item , index) in imgUrls" :key="index">
	   				<image :src="item.imgurl" mode="aspectFill" style="width: 100%;"></image>
	   			</swiper-item>
	</swiper>
	
	<view class="phoneView">
	  <view class="phoneViewbox"> 
	    <view class="phoneViewtitle"> 联系人</view>
	     <input v-model="username" @input="getusername" placeholder="请输入联系人" style="font-size:30rpx; margin-right:30rpx;height:98rpx;width:300rpx;text-align:right;position: absolute; right: 0" ></input>
	   </view>
	    <view class="linview"></view>
	    <view class="phoneViewbox"> 
	    <view class="phoneViewtitle"> 联系电话</view>
	     <input  v-model="phone" @input="getphone" placeholder="请输入联系电话" style="font-size:30rpx; margin-right:30rpx;height:98rpx;width:300rpx;text-align:right;position: absolute; right: 0" ></input>
	   </view>
	  <view class="linview"></view>
	    <view class="phoneViewbox"> 
	    <view class="phoneViewtitle"> 微信号</view>
	     <input  v-model="weixinhao" @input="getweixinhao" placeholder="请输入微信号" style="font-size:30rpx; margin-right:30rpx;height:98rpx;width:300rpx;text-align:right;position: absolute; right: 0" ></input>
	   </view>
	
	   <view class="linview"></view>
	     <view class="phoneViewbox"> 
	    <view class="phoneViewtitle"> 备注</view>
	     <input v-model="remark"  @input="getremark" placeholder="请输入备注" style="font-size:30rpx; margin-right:30rpx;height:98rpx;width:300rpx;text-align:right;position: absolute; right: 0" ></input>
	   </view>
	   <view class="linview" style="margin-bottom:40rpx"></view>
	  </view>
	    <view class="btnView" @click="sureClick">提交</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				    indicatorDots: true, //是否显示面板指示点
				    autoplay: true, //是否自动切换
				    interval: 3000, //自动切换时间间隔
				    duration: 800, //滑动动画时长
				    circular: true, //是否采用衔接滑动
				    imgUrls: [
				    ],
				    username:'',
				    phone:'',
				    remark:'',
				    weixinhao:'',
				
			}
		},
		onLoad(option) {
		this.getBanner();
			
		},
		methods:{
			  getBanner: function () {
			    var _this = this;
			    wx.request({
			      url: 'https://srxadmin.goufeitian.com/index.php/Api/Index/getBanner',
			      data: {},
			      success:function (res) {
			        var content = res.data.data;
					_this.imgUrls = content;
			     
			      }
			    })
			  },
			  getusername:function(e){
				  this.username = e.detail.value;
			   
			   },
			 
			   getweixinhao:function(e){
				    this.weixinhao = e.detail.value;
			  
			   },
			 
			   getphone:function(e){
			      this.phone = e.detail.value;
			   },
			   getremark:function(e){
				   
			     this.remark = e.detail.value;
			   },
			   sureClick(){
					if(this.username ==''){
						uni.showToast({
							icon:'none',
							title:'请输入联系人',
						})
						return;
					}
					
					if(this.phone ==''){
						uni.showToast({
							icon:'none',
							title:'请输入联系电话',
						})
						return;
					}
				
			       var that = this;
				   
			       uni.request({
			           url: 'https://srxadmin.goufeitian.com/index.php/Api/Index/business',
			           data: {
			             emp: that.username,
			             mobile: that.phone,
			             desc:that.remark,
			             wxcode:that.weixinhao,
			           },
			           success: function (res) {
			             if(res.data.status=='1') {
							
			               uni.showToast({
			                 icon:'none',
			                 title:'提交成功'
			               })
			               uni.navigateBack();
			             }
			          
			           },
			       })
		}
		}
	}
</script>

<style>
	
	/* pages/cooperation/cooperation.wxss */
	page {
	  background-color: rgb(245, 245, 245);
	}
	.swiper-item {
	  height: 100%;
	  width: 100%;
	}
	.slide-image{
	   height: 100%;
	  width: 100%;
	}
	.phoneView{
	  margin-top: 30rpx;
	  margin-left: 30rpx;
	  right: 0;
	  left: 0;
	  margin-right: 30rpx;
	  position: absolute;
	
	  background-color: #fff;
	  box-shadow:0px 1px 1px 1px #e1e1e1;
	  border-radius: 20rpx;
	}
	.phoneViewbox{
	  margin-top: 30rpx;
	  width: 100%;
	  height: 100rpx;
	  display: flex;  
	  flex-direction: row; 
	}
	.phoneViewtitle{
	  line-height: 98rpx;
	  margin-left: 40rpx;
	  font-size: 28rpx;
	  width: 180rpx;
	  color: #333333;
	  height: 98rpx;
	  display: flex;
	  flex-direction:column;
	  justify-content: center;
	
	 
	}
	.inputView{
	  margin-right: 20rpx;
	  background-color: #333333;
	  height: 98rpx;
	}
	.linview{
	  margin-left: 30rpx;
	  right: 0;
	  left: 0;
	  margin-right: 30rpx;
	  margin-top: -20rpx;
	  height: 2rpx;
	  background-color: #f5f5f5;
	}
	.line {
	  margin-top: 2rpx;
	  margin-inline: 0rpx;
	  height: 2rpx;
	  background-color: #f5f5f5;
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
	.btnView{
	  position: absolute; 
	  bottom: 20rpx;
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
	.infoText{
	  margin-left: 30rpx;
	  margin-right: 30rpx;
	  margin-top: 30rpx;
	  color:#f65535 ;
	  font-size: 28rpx;
	}
</style>
