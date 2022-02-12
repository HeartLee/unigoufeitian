<template>
	
	<!--pages/bindPhone/binding.wxml-->
	<view>
	<view class="phoneView">
	  <view class="phoneViewbox"> 
	      <view class="phoneViewtitle"> {{oldphoneText}}</view>
	      <view class="linview"></view>
	  </view>
	  <view class="phoneViewbox"> 
	         <input v-model="phoneText" @click="getPhone" placeholder="请输入手机号" style="margin-left:35rpx;font-size: 30rpx;margin-right:300rpx;height:100rpx" ></input>
	      <view class="line"></view>
	    </view>
		<view class="phoneViewbox">
	       <view  class="buttonView"> 
				
	      <button @click="codeClick" :disabled="smsFlag" :style="{'color':sendColor,'font-size':'28rpx','background-color': '#fff'}">{{sendTime}}</button>
	      </view> 
	       <input v-model="code" @click="getcode" placeholder="请输入验证码" style="margin-left:35rpx;font-size: 30rpx;margin-right:300rpx;height:100rpx" ></input>
	    <view class="line"></view>
	  </view>
	</view>
	<view class="btnView" @click="sureClick" >确定</view>
</view>
</template>

<script>
	
	export default {
		data() {
			return {
					oldphoneText:'',
				    sendTime: '获取验证码',
				    sendColor: '#f65535',
				    snsMsgWait: 60,
					smsFlag:false,
				    phoneText:'',
				    mobile:'',
				    code:'',
				
			}
		},
		onLoad(option) {
		console.log(option);
		this.oldphoneText= option.phoneString.substring(0, 3) + '****' + option.phoneString.substring(7);
			
		},
		methods:{
			getcode:function(e){
				this.code = e.detail.value;
			  },
			  getPhone:function(e){
				  	this.phoneText = e.detail.value;
			  },
			  codeClick: function (){
          var that = this;
          uni.request({
              url: 'https://srxadmin.goufeitian.com/index.php/Api/Index/verification',

              data: {
                phone: that.mobile,
              },
              success: function (res) {
                if(res.data.status==1){
                  var inter = setInterval(function() {
                      that.smsFlag = true;
                      that.sendColor = '#cccccc';
                      that.sendTime = that.snsMsgWait + 's后重发';
                      that.snsMsgWait = that.snsMsgWait - 1;

                            if (that.snsMsgWait < 0) {
                              clearInterval(inter)
                      that.smsFlag = false;
                      that.sendColor = '#f65535';
                      that.sendTime = '获取验证码';
                      that.snsMsgWait = 60;
                    }
                  }.bind(that), 1000);
                }
                uni.showToast({
                  icon:'none',
                  title: res.data.info,
                })
              },
          })
		  },
		  sureClick: function (){
		      var  that= this;
		      uni.request({
		        url: 'https://srxadmin.goufeitian.com/index.php/Api/Index/updateMobile',
		        data: {
		          mobile: that.phoneText,
		          openid: localStorage.getItem("opendid"),
		          code:that.code
		        },
		        success: function (res) {
		          if(res.data.status == '1'){
                uni.showToast({
		              icon:'none',
		              title:'换绑成功'
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
	/* pages/bindPhone/binding.wxss */
	
	page {
	  background-color: rgb(245, 245, 245);
	}
	
	.phoneView{
	  position: absolute; 
	  top: 30rpx;
	  left: 30rpx;
	  right: 30rpx;
	  height: 400rpx;
	  background-color: #fff;
	  border-radius:25rpx;
	  box-shadow:0px 1px 1px 1px #e1e1e1;
	}
	.phoneViewbox{
	  width: 100%;
	  height: 130rpx;
	  display: flex;
	  flex-direction:column;
	  justify-content: center;
	}
	.phoneViewtitle{
	  margin-left: 40rpx;
	  font-size: 28rpx;
	  color: #333333;
	  height: 100rpx;
	  display: flex;
	  flex-direction:column;
	  justify-content: center;
	}
	.linview{
	  margin-top: 2rpx;
	  margin-inline: 30rpx;
	  height: 2rpx;
	  background-color: #f5f5f5;
	}
	.line {
	  margin-top: 2rpx;
	  margin-left: 30rpx;
	  margin-right: 30rpx;
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
</style>
