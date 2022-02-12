<template>
	<view>
	<view class="tuiguang">
	    <view class="jinbiView">
	        <view class="moneyView">金币余额（元）</view>
	    </view>
	    <view class="yuename">￥{{teaminfo.money}} </view>
	</view>

    <view class="phoneViewqrcode">
      <div>您的邀请链接为：{{domain}}/#/pages/login/login?userid={{userid}}</div>
      <div style="border-radius:5px;width: 80px;height: 30px;line-height: 30px;background: #007aff;color:#fff;text-align: center" @click="copy">点击复制</div>
    </view>


	<view class="userView">
	    <view class="orderView" @click="gotoMyOrder">
	        <image  src="../../static/dingdan-2.png" mode="widthFix" style="left:10rpx;height:auto"></image>
	        <view class="jiangorder">奖励订单</view>
	        <span class="allorder"	v-if="teaminfo.ordernum != null">订单总数：{{teaminfo.ordernum}}</span>
	        <span class="allorder" v-else>订单总数：0</span>
	        <image src="../../static/arrows.png" mode="widthFix" style="  position: absolute; right:30rpx;"></image>
	    </view>     
	    <view class="team" @click="gotofensi">
	        <image  src="../../static/wodetuandui.png" mode="widthFix" style="left:10rpx; height:auto" ></image>
	        <view class="myteam" >我的团队</view>
	        <span class="allteam" v-if="teaminfo.ordernum != null">团队总人数：{{teaminfo.num}}</span>
	         <span class="allteam" v-else>团队总人数：0</span>
	        <image src="../../static/arrows.png" mode="widthFix" style="  position: absolute; right:30rpx;"></image>
	    </view>
	</view>
	<view class="jieshaoView">
		<!-- <view>
			<image :src="avaimg" mode="aspectFill"  style="width: 200rpx;height: 200rpx;"></image>
		</view> -->
		<span class="invite-friend">您的好友{{nickname}},邀请您吃霸王餐</span>
		<uqrcode style="display: inline-block;" :size="200" ref="uQRCode" :text="`${domain}/#/pages/login/login?userid=${userid}`" />
	</view>
</view>
</template>

<script>
	import QRcode from '@/uni_modules/Sansnn-uQRCode/components/uqrcode/common/uqrcode.js'
	
	export default {
		data() {
			return {
				 teaminfo:{},
        userid:'',
        domain:'',
        url:'',
        inputurl:'',
			}
		},
		onLoad(option) {
			this.isflog = option.isflog;
			console.log(this.isflog);

      let userinfo =  JSON.parse(localStorage.getItem("userinfo"));
	  console.log(userinfo, 'us')
      if(userinfo !=null){
        this.userid =userinfo.id;
		this.nickname = userinfo.nickname;
		// this.avaimg = userinfo.avaimg;
        this.domain = 'https://srx.goufeitian.com';
        // this.inputurl = this.domain+'/#/pages/login/login?userid='+this.userid;
        // console.log(this.inputurl)
        // this.url = 'http://api.k780.com:88/?app=qr.get&data='+this.domain+'/#/pages/login/login?userid='+this.userid
      }
			this.getnumber();
			console.log(QRcode, 'QRcode')
		},
		methods:{
		  getnumber:function(){
		    var  _this = this;
		    uni.request({
		      url: 'https://srxadmin.goufeitian.com/index.php/Api/Index/team',
		      data: {
		          openid: localStorage.getItem("opendid")
		      },
		      success:function (res) {
		        var content = res.data.data;
				_this.teaminfo = content;
		      }
		    })
		  
		  },
		  gotoMyOrder(){
			  uni.navigateTo({
			  	url:'../teamorder/teamorder'
			  })
		  },
		  gotofensi(){
			  uni.navigateTo({
			  	url:'../fensi/fensi'
			  })
		  },
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
	
	/* pages/fenyong/fenyong.wxss */
	page {
	  background-color: rgb(245, 245, 245);
	}
	
	.tuiguang{
	background-color: #f65535;
	height: 160rpx;
	}
	.jinbiView{
	  height: 80rpx;
	  display: flex;
	  justify-content: center;  
	  color: #fff;
	}
	.moneyView{
	  margin-top:30rpx;
	  font-size: 32rpx;
	  color: #fff;
	  display: flex;
	  justify-content: center;  
	}
	.yuename{
	  color: #fff;
	  font-size: 48rpx;
	  display: flex;
	  justify-content: center;  
	}
	.userView{
	
	  columns: #666666;
	  padding: 10px;
	  margin: 40rpx 30rpx 40rpx 30rpx;
	  background-color: #fff;
	
	  border-radius: 20rpx;
	
	}
	.orderView{
	  display: flex;
	  flex-direction: row;
	  align-items: center;
	  margin-bottom: 5rpx;
	  background-color: #fff;
	  line-height: 100rpx;
	}
	.jiangorder{
	  margin-left: 18rpx;
	}
	.orderView image{
	  width:50rpx;
	  margin-left: 8rpx;
	}
	.jiangorder{
	font-size: 32rpx;
	/* font-weight: bold; */
	margin-left: 15rpx;
	}
	.allorder{
	  font-size: 32rpx;
	  position: absolute;
	  right:80rpx;
	}
	.team{
	  display: flex;
	  flex-direction: row;
	  align-items: center;
	  background-color: #fff;
	  margin-bottom: 5rpx;
	  line-height: 100rpx;
	
	}
	.myteam{
	  margin-left: 18rpx;
	  font-size: 32rpx;
	}
	
	.team image{
	  width:60rpx;
	
	}
	.allteam{
	  font-size: 32rpx;
	  position: absolute;
	  right:80rpx;
	}
	.invite{
	  display: flex;
	  flex-direction: row;
	  align-items: center;
	  background-color: #fff;
	  line-height: 100rpx;
	}
	.myinvite{
	  margin-left: 18rpx;
	  font-size: 32rpx;
	}
	.invite image{
	  width:60rpx;
	
	}
	.jieshaoView {
	  background-color: #fff;
	  padding: 10px;
	  margin: 40rpx 30rpx 40rpx 30rpx;
	  border-radius: 20rpx;
	  text-align: center;
	}
	.invite-friend {
		display: inline-block;
		margin-bottom: 10rpx;
	}
	.jieshao{
	  font-size: 32rpx;
	  display: flex;
	  justify-content: center;
	  width: 100%;
	
	}
	.keyjieshao{
	  font-size: 32rpx;
	  text-indent: 70rpx;
	  text-align: left;
	}
  .phoneViewqrcode{
    margin: 20px 15px 20px 15px;
    height:250rpx;
    background-color: #fff;
    border-radius:25rpx;
    padding:20px 20px;
    box-shadow:0px 1px 1px 1px #e1e1e1;
  }

</style>
