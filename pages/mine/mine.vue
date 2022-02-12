<template>
<view>
	<view class="header">
		<view class="userinfo" >
		 <image class="userinfobg" src='../../static/userinfoBg.png'></image>
		 <view class="userinfoView" :style="{'margin-top':statusbar}">
		     <view class="headeinfo"> 
		       <image :src="userInfo.avaimg"   mode="aspectFill" style="width: 200rpx;height: 200rpx;"></image>
		     </view>
			  <view class="userinfoTitle">
			    <view class="headetitle" style="font-weight: bold;font-size: 38rpx;" >{{userInfo.nickname}}</view>
          <view class="headetitle" style="font-size: 28rpx; margin-top:15rpx;" v-if="userInfo.mobile!=null"> 账户名：{{phone}}</view>
			    <view class="headetitle" style="font-size: 28rpx; margin-top:15rpx;" v-if="userInfo.mobile==null"> 暂无设置</view>
			    <view class="bianji" @click="compile" v-if="userInfo.nickname!=null && userInfo.nickname!=''">编辑</view>
			    <view class="bianji" @click="compile" v-else>登录</view>
			  </view>
		</view>
		</view>
		<view class="integralView"  @click="integralClick">
		 <image src="../../static/jifen-2.png" style="width:60rpx;margin-left:60rpx" mode="widthFix"></image>
       <view class="integralViewTitle" style="color: #666666">我的积分</view>
       <view class="integralViewTitle" style="color: #f65535">{{userInfo.money}}分</view>
       <image class="image-jiantou" src="../../static/arrows.png" style="width:30rpx;margin-right: 60rpx;" mode="widthFix"></image>
		</view>
	</view>
		<image  src="../../static/kvip.jpeg" mode="widthFix" v-if="isvip!='1'" style="width: calc(100% - 60rpx);margin-left: 30rpx;margin-top: 30rpx;" @click="buyvip"></image>
		<view class="menuView">
		  <div class="menuViewlist" @click="orderClick">
			 <image src="../../static/myorder.png" style="width:40%" mode="widthFix"></image>
			<view class="menuViewtitle" >我的订单</view>
		   </div>
		  <div class="menuViewlist" @click="fenyong">
			<image src="../../static/qianbao-2.png" style="width:40%" mode="widthFix"></image>
			<view class="menuViewtitle">分佣查看</view>
		  </div>
		  <div class="menuViewlist" @click="payCode"> 
			<image src="../../static/erweima.png" style="width:40%" mode="widthFix"></image>
			<view class="menuViewtitle">收款二维码</view></div>
		   <div class="menuViewlist" @click="compile"> 
			 <image src="../../static/gerenziliao.png" style="width:40%" mode="widthFix"></image>
			  <view class="menuViewtitle">个人资料</view>
			</div>
		  </view>
		  <view class="lineView"></view>
			<block v-for="(item,index) in items" :key="index">
			    <view class="list-item"  @click='dunHuanAction(item.ID)' >
			      <image class="item-image" :src="item.uristring" mode="widthFix" style="width:80rpx"></image>
			      <text class="item-text">{{item.name}}</text>
			      <image class="image-jiantou" src="../../static/arrows.png"></image>
			    </view>
	 
			</block>
</view>
	
</template>

<script>
export default {
	data() {
		return {
		userInfo:{},
		       phone:'',
		       statusbar: 0,
		       showModal: false,
		       uid: '',
		       money: '0',
		       ziti: '',
		       isvip:'',
		       login_flag: false,
		       radioItems: [
		           {name: '微信零钱', value: '1', checked: true}
		       ],
		       items: [
		           {name: '推广有礼', uristring: '../../static/user_share.png', ID: '7'},
		           {name: '忘记支付密码', uristring: '../../static/mima.png', ID: '2'},
		           {name: '绑定支付宝', uristring: '../../static/zhifubao.png', ID: '3'},
		           {name: '帮助中心', uristring: '../../static/bangzhu.png', ID: '4'},
		           {name: '通知消息', uristring: '../../static/mess.png', ID: '5'},
		           {name: '商务合作', uristring: '../../static/hezuo.png', ID: '6'},
				  
				   {name: '退出登录', uristring: '../../static/user_share.png', ID: '8'},
		       ],
		       tixiantype: "1",
		       tixianmoney: '',
		       uname: '',
		       touxiang: '',
		       arrows: 0,
			}
	},
	onLoad(option) {
		var that = this;
		let systemInfo = wx.getSystemInfoSync()
		let pxToRpxScale = 750 / systemInfo.windowWidth;
		let ktxStatusHeight = systemInfo.statusBarHeight * pxToRpxScale
		let navigationHeight = 44 * pxToRpxScale
		this.statusbar = ktxStatusHeight + navigationHeight ;
		this.statusbar = this.statusbar +'rpx';
		this.getPersoninfo();
	},
	methods:{
		  //收款二维码
		    payCode: function (e) {  
				var that = this;
		        if(that.userInfo.mobile==null){
              uni.navigateTo({
                url:"../shoujihao/shoujihao"
              })
		        }else{
		            var option = {
		                weixin_url:this.userInfo.weixin_url,
		                zhifubao_url:this.userInfo.zhifubao_url
		            }
		            uni.navigateTo({
		                url: '../paycode/paycode?option='+JSON.stringify(option)
		            })
		       }
		    },
		  orderClick: function (e) {
				uni.switchTab({
					url:'../order/order'
				})
		    },
		    fenyong: function (e) {
		        var that = this;
		        if(that.userInfo.mobile==null){
              uni.navigateTo({
						url:'../shoujihao/shoujihao'
					})
		        }else{
              uni.navigateTo({
						url:'../fenyong/fenyong'
				   })
		    }
		    },
       buyvip:function (e) {
         uni.navigateTo({
          url:'../vipbuy/vipbuy'
         })
		    },
		  integralClick: function (e) {
		        var that = this;
		        if(that.userInfo.mobile==null){
		            uni.navigateTo({
		
		                url: '../shoujihao/shoujihao'
		            })
		        }else{
		        uni.navigateTo({
		            url: '../integral/integral'
		        });
		    }
		    },
		   compile: function (e) {
		        var that = this;
		        if(that.userInfo.mobile==null){
              uni.navigateTo({
		                url: '/pages/shoujihao/shoujihao'
		            })
		        }else{
					uni.navigateTo({
						url:'../compile/compile'
					})
		    }
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
	                    if(res.data.data.mobile==null)
	                    {
                        that.userInfo = res.data.data;
                        that.isvip = res.data.data.isvip;
                      }else{
                        that.userInfo = res.data.data;
                        that.isvip = res.data.data.isvip;
                        that.phone = res.data.data.mobile.substring(0, 3) + '****' + res.data.data.mobile.substring(7);
                      }
	                } else {

	                }
	            },
	        })
	    },
		    //列表点击
		    dunHuanAction: function (e) {
		        if(this.userInfo.nickname==null ||this.userInfo.nickname==''){

		        }
		        var that = this;
		        if(that.userInfo.mobile==null){

		        }else{
		        if (e == 1) {
		            uni.navigateTo({
		                url: '../login/forgetpass' 
		            })
		        }
		        if (e == 2) {
		            uni.navigateTo({
		                url: '../paypassword/paypassword?phonetext=' + this.userInfo.mobile
		            })
		        }
		        if (e == 3) {
		            var option = {
		                zhifubao_code:this.userInfo.zhifubao_code,
		                zhifubao_name:this.userInfo.zhifubao_name
		            }
		            uni.navigateTo({
		            	url:'../bingZhifubao/bingZhifubao?option='+JSON.stringify(option)
		            })
		        }
		        if (e == 4) {
		            uni.navigateTo({
		
		               url: '../help/help'
		            })
		        }
		        if (e == 5) {
                uni.navigateTo({
                  url: '../message/message'
                })
		        }
		        if (e == 6) {
		            uni.navigateTo({
					        url:'../cooperation/cooperation'
		            })
		        }
		        if (e == 7) {
                uni.navigateTo({
                  url:'../fenyong/fenyong'
		            })
		        }
            if (e == 8) {
                uni.navigateTo({
                    url: '../login/loginout'
                })
            }
		        }

		    },
	},
}
</script>

<style>
	page {
	  background-color: #fff;
	}
	image{
	  height: auto;
	}
	.userinfo{
	  /* background-color: #f65535; */
	  width: 100%;
	  height: 520rpx;
	  border-bottom-left-radius: 20rpx;
	  border-bottom-right-radius: 20rpx;
	  /* background-color: tomato; */
	  background-image: image(src);
	  /* margin-top: 0rpx; */
	}
	.userinfo .userinfobg{
	  position: absolute;
	  display: block;
	  width: 100%;
	  z-index: -999;
	  height: 520rpx;
	  top: 0rpx;
	  border-bottom-left-radius: 20rpx;
	  border-bottom-right-radius: 20rpx;
	}
	.userinfo .userinfoView{
	  width: 100%;
	  height: 240rpx;
	  margin-top:60rpx;
	  
	  display: flex;
	  /*row 横向  column 列表  */
	  flex-direction: row;
	}
	.userinfoView .headeinfo{
	  width: 200rpx;
	  height: 200rpx;
	  border-radius: 50%;
	  /* background-color: #000; */
	  margin-left: 70rpx;
	}
	.userinfoView .headeinfo image{
	  width: 200rpx;
	  border-radius: 50%;
	}
	
	.userinfoView .userinfoTitle{
	  margin-left:30rpx;
	  /* margin-right: 80rpx; */
	  height:100%;
	  display: flex;
	  /*row 横向  column 列表  */
	  flex-direction: column;
	}
	.userinfoView .headetitle{
	  /* display: inline-block; */
	  /* position: absolute; */
	  margin-left: 20rpx;
	  margin-top:20rpx;
	  color: #000e;
	}
	.userinfoView .bianji{
	  margin-top:30rpx;
	  font-size: 28rpx;
	  width: 140rpx;
	  height: 60rpx;
	  background-color: #fff;
	  margin-left:10rpx;
	  border-radius: 30rpx;
	  color: #f65535; 
	  line-height: 60rpx;
	  text-align: center;
	}
	.integralView{
	  margin-top: -120px;
	  margin-right: 30rpx;
	  margin-left: 30rpx;
	  height: 120rpx;
	  background-color: #fff;
	  border-radius:70rpx;
	  box-shadow:0px 1px 1px 1px #eee;
	  display: flex;
	  /*row 横向  column 列表  */
	  flex-direction: row;
	  align-items: center;
	}
	.integralViewTitle{
	 margin-left: 45rpx;
	 font-size: 28rpx;
	}
	.menuView{
	  display: flex;
	  align-items: center;
	  justify-content: center;
	  margin-left: 0rpx;
	  margin-right:0rpx ;
	  margin-top: 30rpx;
	
	}
	.lineView{
	  margin-top: 20rpx;
	  width: 100%;
	  height: 20rpx;
	  background-color: #f5f5f5;
	}
	.menuViewlist{
	  width: 25%;
	  display: flex;
	  flex-direction: column;
	  align-items: center;  
	  justify-content: center;
	}
	.menuViewtitle{
	  margin-top: 20rpx;
	  font-size: 24rpx;
	color: #080808;
	}
	.list-item {
	  display: flex;
	  flex-direction: row;
	  align-items: center;
	  width: 100%;
	  height: 100rpx;
	  margin-top: 10rpx;
	
	  position: relative; /*父元素位置要设置为相对*/
	  border-bottom:1px  #ededed solid;
	}
	button::after{
	
	  border: 0;
	  
	}
	.item-image {
	  
	  width: 60rpx;
	  margin: 20rpx;
	  margin-left: 50rpx;
	}
	
	.item-text {
	  color: #333333;
	  font-size: 30rpx;
	  margin-left: 15rpx;
	}
	
	.image-jiantou {
	  width: 30rpx;
	  height: 30rpx;
	  position: absolute; /* 要约束所在位置的子元素的位置要设置成绝对 */
	  right: 0; /* 靠右调节 */
	  margin-right: 10rpx;
	}
	
	.line {
	  width: 100%;
	  height: 2rpx;
	  background: #f5f5f5;
	  margin-left: 0rpx;
	}
	
	.forminfo{margin-top:2rem}
</style>
