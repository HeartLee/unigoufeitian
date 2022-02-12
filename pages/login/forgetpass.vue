<template>
	
	<!--pages/bindPhone/binding.wxml-->
	<view>
	<view class="phoneViewforget">
	  
	  <view class="phoneViewbox"> 
	      <input v-model="phoneText"   placeholder="请输入手机号" style="margin-left:35rpx;font-size: 30rpx;margin-right:300rpx;height:100rpx" ></input>
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
	  
	  
	  
	</view>
	<view class="btnView" @click="sureClick" >修改密码</view>
</view>
</template>

<script>
	
	export default {
		data() {
			return {
				    phoneText:'',
				    password:'',
					password2:'',
				
			}
		},
		onLoad(option) {
	 
			
		},
		methods:{
			gotoLogin: function(){
				uni.navigateTo({
					url:'../login/login'
				})
			},
		 
		  sureClick: function (){
			  if(this.password != tihs.password2){
				  uni.showToast({
					  title: '二次输入密码不正确',
					  duration: 2000
				  });
				  return;
			  }
		      var  that= this;
		      uni.request({
		        url: 'https://srxadmin.goufeitian.com/index.php/Api/Index/updatePassword',
		        data: {
		          mobile: that.phoneText,
		          password: that.password,
		        },
		        success: function (res) {
		          if(res.data.status == '1'){
		            uni.showToast({
						  title: '注册成功',
						  duration: 2000
		            });
		            uni.navigateTo({
		            	url:'../login/login'
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
	  background-color: rgb(245, 245, 245);
	}
	
	.phoneViewforget{
	  position: absolute; 
	  top: 30rpx;
	  left: 30rpx;
	  right: 30rpx;
	  height: 450rpx;
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
