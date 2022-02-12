<template>
	
	<!--pages/bindZhifubao/bingZhifubao.wxml-->
	<!--pages/bindPhone/binding.wxml-->
	<view>
	<view class="phoneView">
	  <view class="phoneViewbox"> 
	    <view class="phoneViewtitle"> {{phoneText}}</view>
	    <view class="linview"></view>
	  </view>
	  
	  <view class="phoneViewbox" style="position: relative;">
	         <input v-model="verify"   placeholder="请输入图片验证码" style="margin-left:35rpx;font-size: 30rpx;margin-right:300rpx;height:100rpx" ></input>
	         <view class="line"></view>
	  			 <view style="position: absolute;right:10rpx;height: 5px;top:5px;">
	  				   <img :src="versrc" @click="changeCode">
	  				 
	  		  </view>
	  </view>
	  
	  
	  <view class="phoneViewbox"> 
	         <view  class="buttonView"> 
			
	        <button @click="codeClick" :disabled="smsFlag" :style="{'color':sendColor,'font-size':'28rpx','background-color': '#fff'}">{{sendTime}}</button>
	        </view> 
	         <input v-model="code" placeholder="请输入短信验证码" style="margin-left:35rpx;font-size: 30rpx;margin-right:300rpx;height:100rpx" ></input>
	      <view class="line"></view>
	    </view>
	  
	  
	    <view class="phoneViewbox"> 
	       <input v-model="password"   placeholder="请输入新密码" style="margin-left:35rpx;font-size: 30rpx;margin-right:35rpx;height:100rpx" ></input>
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
			    sendTime: '获取验证码',
			    sendColor: '#f65535',
			    snsMsgWait: 60,
				smsFlag:false,
			    phoneText:'',
			    mobile:'',
			    code:'',
				password:'',
				verify:'',
				versrc:'https://srxadmin.goufeitian.com/index.php/Api/Index/verify'
			
		}
	},
	onLoad(option) {
		      this.phoneText=option.phonetext.substring(0, 3) + '****' + option.phonetext.substring(7);
		      this.mobile=option.phonetext;
		
	},
	methods:{

		codeClick: function (){
	    var that = this;
		
		if(that.verify == null || that.verify == ''){
						  uni.showToast({
									icon:"none",
						  	      title:'请先输入图文验证码，然后再获取短信验证码！',
						  	      duration: 2000
						  	  });
						  return;
		}	
					
					
	    uni.request({
	        url: 'https://srxadmin.goufeitian.com/index.php/Api/Index/verification',
	        data: {
	          phone: that.mobile,
			  verify:that.verify
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
	  changeCode :function(){
	  		this.versrc ='https://srxadmin.goufeitian.com/index.php/Api/Index/verify?'+new Date().getTime();
	   },
	   sureClick: function (){
	      if (!(/(^[0-9]*$)/.test(this.password))) {
	          uni.showToast({
	            title: '密码为6位数字',
	            icon:"none"
	          })
	          return
	      }
	      if (this.password.length!=6) {
	        uni.showToast({
	          title: '密码为6位数字',
	          icon:"none"
	        })
	        return
	    }
	      if(this.code=='') {
	        uni.showToast({
	          title: '请输入验证码',
	          icon:"none"
	        })
	        return
	      }
	      var  that= this;
	      uni.request({
	        url: 'https://srxadmin.goufeitian.com/index.php/Api/Index/updatePaypassword',
	        data: {
	          paypassword: that.password,
	          openid:localStorage.getItem("opendid"),
	          code:that.code
	        },
	        success: function (res) {
	          if(res.data.status=='1'){
              uni.showToast({
	              icon:"none",
	              title: '修改支付密码成功',
	            })
	          }else{
              uni.showToast({
                icon:"none",
                title: res.data.info,
              })
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
	  height: 550rpx;
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
	button::after {
	  border: none;
	 
	 }
	.buttonView button{
	  padding: 0rpx;
	  background-color: #fff;
	  color: #f65535;
	  font-size: 28rpx;
	}
</style>
