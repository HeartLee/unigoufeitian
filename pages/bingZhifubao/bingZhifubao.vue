<template>
	<view>
		<view class="phoneView">
		  <view class="phoneViewbox"> 
		    <view class="phoneViewtitle"> 真实姓名</view>
		     <input v-model="username" @click="getzhifubaoname" placeholder="请输入真实姓名" style="font-size:30rpx; margin-right:30rpx;height:98rpx;width:300rpx;text-align:right;position: absolute; right: 0" ></input>
		   </view>
		    <view class="linview"></view>
		    <view class="phoneViewbox"> 
		    <view class="phoneViewtitle"> 支付宝账号</view>
		     <input v-model="zhifubaocode" @click="getzhifubaocode" placeholder="请输入支付宝账号" style="font-size:30rpx; margin-right:30rpx;height:98rpx;width:300rpx;text-align:right;position: absolute; right: 0" ></input>
		   </view>
		  </view>
		  <view class="infoText">*为了您的款及时正确到账，您需要提供您的支付宝收款账号和真实姓名！</view>
		  <view class="btnView" @click="sureClick">保存</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				    zhifubaocode:'',
				    username:''
				
			}
		},
		onLoad(option) {
			 var option = JSON.parse(option.option);
			 this.zhifubaocode = option.zhifubao_code;
			 this.username = option.zhifubao_name;
			
		},
		methods:{
			getzhifubaocode:function(e) {
				this.zhifubaocode = e.detail.value;
			  },
			  getzhifubaoname:function(e){
				  this.username = e.detail.value;
			  },
			  sureClick:function(e){
			    var  that= this;
			    uni.request({
			      url: 'https://srxadmin.goufeitian.com/index.php/Api/Index/updateAlipay',
			      data: {
			        zhifubao_name: that.username,
			        openid:localStorage.getItem("opendid"),
			        zhifubao_code:that.zhifubaocode
			      },
			      success: function (res) {
			        if(res.data.status=='1'){
                uni.showToast({
			            icon:"none",
			            title: '绑定成功',
			          })
                uni.navigateBack();
			        }
			      
			      },
			  })
			  },
		},
	}
</script>

<style>
	/* pages/bindZhifubao/bingZhifubao.wxss */
	page {
	  background-color: rgb(245, 245, 245);
	}
	
	.phoneView{
	  margin-top: 30rpx;
	  height: 200rpx;
	  background-color: #fff;
	  box-shadow:0px 1px 1px 1px #e1e1e1;
	}
	.phoneViewbox{
	
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
	  margin-top: 0rpx;
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
