<template>
	
	<view>
	<view class="phoneViewreg">
      <view style="margin-top: 10px;margin-left: 20px;">您的上级推荐人id为：{{userid}}</view>

      <view class="phoneViewbox">
             <input v-model="phoneText"   placeholder="请输入手机号" style="margin-left:35rpx;font-size: 30rpx;margin-right:300rpx;height:100rpx" ></input>
             <view class="line"></view>
      </view>
	  
	  <view class="phoneViewbox" style="position: relative;">
             <input v-model="verify"   placeholder="请输入验证码" style="margin-left:35rpx;font-size: 30rpx;margin-right:300rpx;height:100rpx" ></input>
	         <view class="line"></view>
			 <view style="position: absolute;right:10rpx;height: 5px;top:5px;">
				   <img :src="versrc" @click="changeCode">
				 
			 </view>
	  </view>
	  
	  
	  
      <view class="phoneViewbox">

         <view  class="buttonView">
            <button @click="codeClick" :disabled="smsFlag" :style="{'color':sendColor,'font-size':'28rpx','background-color': '#fff'}">{{sendTime}}</button>
         </view>
         <input v-model="code"  placeholder="请输入验证码" style="margin-left:35rpx;font-size: 30rpx;margin-right:300rpx;height:100rpx" ></input>
         <view class="line"></view>
        </view>

      <view class="phoneViewbox">
             <input v-model="password"   placeholder="请输入密码" style="margin-left:35rpx;font-size: 30rpx;margin-right:300rpx;height:100rpx" ></input>
             <view class="line"></view>
      </view>

      <view class="phoneViewbox">
             <input v-model="password2"   placeholder="请再次输入密码" style="margin-left:35rpx;font-size: 30rpx;margin-right:300rpx;height:100rpx" ></input>
             <view class="line"></view>
      </view>

      <view class="phoneViewbox">
        <input v-model="name"   placeholder="请输入姓名" style="margin-left:35rpx;font-size: 30rpx;margin-right:300rpx;height:100rpx" ></input>
        <view class="line"></view>
      </view>
      <div style="float:right;margin-right:30rpx;margin-top:30rpx;color:#999;" @click="gotoLogin">
          <span>我要登录</span>
      </div>
	  
	</view>
	<view class="btnView" @click="sureClick" >注册</view>
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
				    code:'',
            password:'',
            password2:'',
            userid:'0',
            name:'',
			verify:'',
			versrc:'https://srxadmin.goufeitian.com/index.php/Api/Index/verify'
			}
		},
		onLoad(option) {
		  if(this.$route.query.userid){
		    this.userid = this.$route.query.userid;
      }
		},
		methods:{
			gotoLogin: function(){
				uni.navigateTo({
					url:'../login/login'
				})
			},
			codeClick: function (){
				var that = this;
				uni.request({
		        url: 'https://srxadmin.goufeitian.com/index.php/Api/Index/verification',
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
			  if(!this.password || !this.name || !this.phoneText || !this.code){
          uni.showToast({
            icon:"none",
            title: '输入不能为空',
          });
          return;
        }
			  if(this.password != this.password2){
				  uni.showToast({
              icon:"none",
				      title: '二次输入密码不正确',
				  });
				  return;
			  }
		      var  that= this;
		      uni.request({
		        url: 'https://srxadmin.goufeitian.com/index.php/Api/Index/reg',
		        data: {
		          mobile: that.phoneText,
		          openid: localStorage.getItem("opendid"),
		          code:that.code,
              name:that.name,
              pid:that.userid
		        },
		        success: function (res) {
		          if(res.data.status == '1'){

                uni.showModal({
                  title: '提示',
                  content: '注册成功',
                  confirmText: '去登录',
                  confirmColor: '#f65535',
                  success (res) {
                    if (res.confirm) {
                      uni.navigateTo({
                        url:'../login/login'
                      })

                    }
                  }
                })
		          }else{
                uni.showToast({
                  icon:"none",
                  title: res.data.info,
                  duration: 2000
                });
              }
		        },
		    })
		    },
		},
	}
</script>

<style>
	page {
	  background-color: rgb(245, 245, 245);
	}
	
	.phoneViewreg{
	  position: absolute; 
	  top: 30rpx;
	  left: 30rpx;
	  right: 30rpx;
	  height: 830rpx;
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
