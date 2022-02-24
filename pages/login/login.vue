<template>
<view class="main">
	 <view class="titleView">
		 欢迎登录惠食天下，探店优质美食
	 </view>
     <view v-if="!!userid" style="margin-top: 10px;margin-left: 40px;">您的上级推荐人id为：{{userid}}</view>
   
	 <view class="contentView">
	    <view class="image" >
	        <image   src="../../static/shoujihao.png" style="width:100%" mode="widthFix">
	        </image>
	    </view>
	    <view class="inputView" >
	        <input v-model="phoneText"  placeholder="请输入手机号" style="height: 80rpx;font-size: 32rpx;">
	        </input>
	    </view>
	</view>
	
	<!-- <view class="contentView">
	    <view class="image" >
	        <image   src="../../static/shoujihao.png" style="width:100%" mode="widthFix">
	        </image>
	    </view>
	    <view class="inputView" >
	       <input v-model="verify"  placeholder="请输入图片验证码" style="height: 80rpx;font-size: 32rpx;">
	   </input>
	   </view>
	   <img :src="versrc" @click="changeCode">
	</view> -->
	

	<view class="contentView">
	    <view class="image" >
	        <image   src="../../static/mima.png" style="width:100%" mode="widthFix">
	        </image>
	    </view>
	    <view class="inputView" >
			<view  class="buttonView">				
			<button @click="codeClick" :disabled="smsFlag" :style="{'color':sendColor,'font-size':'28rpx', 'height':'80rpx', 'background-color': '#fff', 'border-radius': '50rpx'}">{{sendTime}}</button>
			</view> 
	        <input v-model="code" placeholder="短信验证码" maxlength="6" style="height: 80rpx;width:200rpx;font-size: 32rpx;">
	        </input>
	    </view>
		
	</view>
	
<!-- 	<view style="color:#000;font-size: 12px;margin-top: 20px;padding:0 40px;line-height: 30px;">
		①图片验证码不区分大小写<span style="color:red">(安全无痛)</span></br>
		②切换一张清晰可辨别的再输入即可</br>
		③短信验证码有效期30天，一次收到，持续有效</br>
		④首次登录后，关注置顶公众号:惠食天下外卖</br>

	</view> -->
	
	
	<view style="margin-bottom:30px;height: 30px;display:none ;">
		<view @click="gotoReg" style="float:right;margin-top: 20px;margin-right: 40px;color:#999">我要注册</view>
	</view>
	<view class="loginbutton">
		 <button class="loginbg"  style="font-size: 36rpx;"   lang="zh_CN" @click="sureClick">登录</button>
	</view>
</view>
</template>

<script>
	
	export default {
		data() {
			return {
					sendTime: '获取短信验证码',
					sendColor: '#f65535',
					snsMsgWait: 60,
					smsFlag:false,
				    phoneText:'',
				    code:'',
					verify:'',
					versrc:'https://srxadmin.goufeitian.com/index.php/Api/Index/verify',
					userid: 0,
				
			}
		},
		onLoad(option) {
			if(this.$route.query.userid){
				this.userid = this.$route.query.userid;
			}
		},
		methods:{
			codeClick: function (){
			  var that = this;
			  // if(that.verify == null || that.verify == ''){
				 //  uni.showToast({
					// 		icon:"none",
				 //  	      title:'请先输入图文验证码，然后再获取短信验证码！',
				 //  	      duration: 2000
				 //  	  });
				 //  return;
			  // }
			
			
			  uni.request({
			      url: 'https://srxadmin.goufeitian.com/index.php/Api/Index/verification_login',

			      data: {
			        phone: that.phoneText,
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
					  } else  if(res.data.status==0){
						  uni.showToast({
						    icon:"none",
						    title:res.data.info,
						    duration: 2000
						  });
			        } else{
                uni.showToast({
                  icon:"none",
                  title:'未知错误，请联系管理员',
                  duration: 2000
                });
              }
			        uni.showToast({
			          icon:'none',
			          title: res.data.info,
			        })
			      },
			  })
			},
		  gotoReg :function(){
			  uni.navigateTo({
			  	url:'../login/reg'
			  })
		  },
		 changeCode :function(){
			 this.versrc ='https://srxadmin.goufeitian.com/index.php/Api/Index/verify?'+new Date().getTime();
		  },
		  sureClick: function (){
		      var  that= this;
			  if(!(that.phoneText && that.code)){
				   uni.showToast({
				  		icon:"none",
				   	      title:'请输入手机号、短信验证码！',
				   	      duration: 2000
				   	  });
			  	return;
			  }
		      uni.request({
		        url: 'https://srxadmin.goufeitian.com/index.php/Api/Index/loginh5',
		        data: {
				  userid:that.userid,	
		          tele: that.phoneText,
		          password:  that.code,
		        },
		        success: function (res) {
				  if(res.data.status == 0){
					  uni.showToast({
                icon:"none",
					      title: res.data.data,
					      duration: 2000
					  });
				  }else{
					  localStorage.setItem("token",res.data.data.token);
					  localStorage.setItem("opendid",res.data.data.openid);
					  localStorage.setItem("userinfo",JSON.stringify(res.data.data));
					  uni.switchTab({
							url:'../index/index'
						})
				  }
		        },
		    })
		    },
		},
	}
</script>

<style>	
	page {
	  background: url(../../static/loginback.png) repeat;
	  background-size:cover; 
	}
	.background{
	    width:100%;  
	    height:100%;  /**宽高100%是为了图片铺满屏幕 */
	    z-index:-1;
	    position: absolute;
	}
	.header{
	  padding:30rpx;
	}
	.titleView{
		padding-top:60rpx;
		margin-left: 80rpx;
		color: red;
		font-size: 40rpx;
		margin-bottom: 80rpx;
	}
	.loginbg{
	 border-radius: 50rpx;
	  border:1px #f65535 solid;
	  height: 100rpx;
	  color:#fff;
	  line-height: 100rpx;
	  background-color: #f65535;
	}
	.contentView{
	  margin: 50rpx 80rpx 0rpx 80rpx;
	   height: 100rpx;
	 border: solid 1px #f65535;
	border-radius: 50rpx;
		display: flex;
	  flex-direction: row;
	}
	.contentView .image{
	  margin-top: 25rpx;
	  margin-left: 30rpx;
	  width: 50rpx;
	  height: 50rpx;
	}
	.contentView .lineView{
		margin-left: 20rpx;
		height: 50rpx;
		color: #CCCCCC;
		width: 1px;
		margin-top: 25rpx;
	}
	.contentView .inputView{
		margin-top: 10rpx;
	  margin-right: 10rpx;
	  color: #333333;
	  font-size: 26rpx;
	  margin-left: 15rpx;
	  align-items: center;
	  height: 100rpx;
	  line-height: 100rpx;
	}
	.inputView .uni-input-placeholder {
		color: #423e3e;
	}
	.buttonView{
	  display: flex;
	  height: 80rpx;
	  justify-content: center;
	  align-items: center;
	  /* width: 200rpx; */
	  position: absolute; 
	  right: 0; /* 靠右调节 */
	  margin-right: 90rpx;
	  color: #f65535;
	}
	button::after {
	  border: none;
	 
	 }

	.line{height: 2rpx;background: #cccccc;margin-top: 0rpx;margin-left: 40rpx;margin-right: 40rpx;}
	.loginbutton{
		margin-top:80rpx;
		text-align: center;
		margin-left: 80rpx; 
		margin-right: 80rpx;
		/* background-color: #fff; */
		}
</style>
