<template>
	<view>
		<view class="phoneViewqrcode">
      <div>您的邀请链接为：{{domain}}/#/pages/login/reg?userid={{userid}}</div>
      <div style="border-radius:5px;width: 80px;height: 30px;line-height: 30px;background: #007aff;color:#fff;text-align: center" @click="copy">点击复制</div>
      <div style="text-align: center"> <img :src="url"></div>
		 </view>

	 </view>
</template>

<script>
	
	export default {
		data() {
			return {
				userid:'',
				domain:'',
        url:'',
        inputurl:'',
			}
		},
		onLoad(option) {
			let userinfo =  JSON.parse(localStorage.getItem("userinfo"));
			if(userinfo !=null){
				this.userid =userinfo.id
				this.domain = 'https://srx.goufeitian.com';
				this.inputurl = this.domain+'/#/pages/login/reg?userid='+this.userid;
				console.log(this.inputurl)
				this.url = 'http://api.k780.com:88/?app=qr.get&data='+this.domain+'/#/pages/login/reg?userid='+this.userid
			}
		
		},
		methods:{
      copy() {
        let value = this.inputurl
        //提示模板
        uni.showModal({
          content: value, //模板中提示的内容
          confirmText: '复制内容',
          success: () => { //点击复制内容的后调函数
            //uni.setClipboardData方法就是讲内容复制到粘贴板
            // API `setClipboardData` is not yet implemented
            //意思是H5端没有这个接口！！！
            uni.setClipboardData({
              data: value, //要被复制的内容
              success: function() {
                //重点~做笔记
                //在success中加入uni.hideToast()可以解决
                uni.hideToast({
                  title: '复制成功',
                  duration: 2000,
                  icon: 'none'
                });
                //以下就可自定义操作了~
              },
              fail: function(err) {
                uni.showToast({
                  title: '复制失败',
                  duration: 2000,
                  icon: 'none'
                });
              }
            });
          }
        });
      }
		},
	}
</script>

<style>
	page {
	  background-color: rgb(245, 245, 245);
	}

	.phoneViewqrcode{
	  position: absolute; 
	  top: 30rpx;
	  left: 30rpx;
	  right: 30rpx;
	  height: 550rpx;
	  background-color: #fff;
	  border-radius:25rpx;
	  padding:20px 20px;
	  box-shadow:0px 1px 1px 1px #e1e1e1;
	}
	 
</style>
