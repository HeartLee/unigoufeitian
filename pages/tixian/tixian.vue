<template>
	<view>
		<view class="contentView">
		  <view class="codeView">
		    <view class="title">提现到我的提款二维码</view>
		    <view class="imageView">
		      <image :src="userInfo.zhifubao_url" mode="aspectFit"></image>
		    </view>
		  </view>
		  <view class="jifenView">
		    <view style="font-size:24rpx">提现积分</view>
		    <input placeholder="请输入提现积分"  v-model="money"></input>
        <!-- <input placeholder="请输入支付密码"  v-model="password"></input> -->
		   <view class="tishiView">
		    <view >当前积分:{{userInfo.money}}分,</view>
		    <view style="color:red;margin-left:20rpx" @click="alltixian">全部提现</view>
		   </view>
		  </view>
		      <view class="btnView">
		          <button @click="txmoney">提现</button>
		      </view>
		</view>
		<view class="inText">
		<view class="tixianText" style="color:red;font-weight:bold" >提现须知</view>
		<view  style="white-space:pre-wrap;margin-top: 30rpx;" >
			1、因微信扫码支付限额100笔当日，最低提现金额修正为10积分（希望理解）；
			</view>
			<view  style="white-space:pre-wrap">
			2、提现积分次日到账；
			</view>
			<view  style="white-space:pre-wrap">
			3、提现订单处理时间为每天10:00审核处理昨日提现；
			</view>
			<view  style="white-space:pre-wrap">
			4、周六周日不处理提现订单；
			</view>
			<view  style="white-space:pre-wrap">
			5、提现无任何手续费；
			</view>
		</view>
	</view>

</template>

<script>
	
	export default {
		data() {
			return {
				   money:"",
				    allmoney:'',
				    userInfo:{},
				    isShowPwdWrap: false,
				    // password: '',
				    inputFocus: true,
				    check:true,
			}
		},
		onShow() {
			this.getPersoninfo();
		},
		onLoad(option) {

		},
		methods:{
			getPersoninfo: function (options) {
			    var that = this;
			    uni.request({
			        url: 'https://srxadmin.goufeitian.com/index.php/Api/Index/getPersoninfo',
			        data: {
			           openid: localStorage.getItem("opendid")
			        },
			        success: function (res) {
			            if (res.data.status == '1') {
							 that.userInfo = res.data.data;
							 that.allmoney=res.data.data.money;
			            } else {
			                uni.showToast({
			                    title: res.data.info,
			                    icon: 'none',
			                    duration: 2000
			                })
			            }
			        },
			    })
			}, 
			txmoney:function () {
			     var _this = this;

			     if(_this.userInfo.zhifubao_url == null){
			       var option = {
			         zhifubao_url:_this.userInfo.zhifubao_url
			     }
             uni.showModal({
			         title: '提示',
			         content: '您还没设置收款二维码',
			         confirmText: '去设置',
			          confirmColor: '#f65535',
			         success (res) {
			           if (res.confirm) {
                   uni.navigateTo({
			               url: '../paycode/paycode?option='+JSON.stringify(option)
			             })
			 
			           }
			         }
			       })
			       return
			     }
			     // if(_this.userInfo.paypassword == null){

        //      uni.showModal({
			     //     title: '提示',
			     //     content: '您还没设置支付密码',
			     //     confirmText: '去设置',
			     //     confirmColor: '#f65535',
			     //     success (res) {
			     //       if (res.confirm) {
        //            uni.navigateTo({
			     //           url: '../paypassword/paypassword?phonetext=' + _this.userInfo.mobile
			     //         })
			     //       }
			     //     }
			     //   })
			     //   return
			     // }
			     if(_this.money == ''){
             uni.showToast({
			         icon:"none",
			         title:'请输入提现积分'
			       })
			       return
			     }
            _this.sureClick();
			},

				
				sureClick:function() {
				    var _this = this;
				    if(this.check){
				      uni.request({
				      url: 'https://srxadmin.goufeitian.com/index.php/Api/Index/tixian',
				      data: {
				        money:_this.money,
				         openid: localStorage.getItem("opendid"),
				        // password:_this.password
				      },
				      success:function (res) {
                _this.inputFocus = false;
                _this.isShowPwdWrap = false;
                _this.check = true;
                // _this.password = '';

				      
				       if(res.data.status=='1'){
				        uni.redirectTo({
				          url:'./tixianresult'
				        })
				       }else{
				        uni.showToast({
				          icon:"none",
				          title:res.data.info,
				          duration: 2000
				        })
				       }
				      }
				    })
				  }
				  _this.check = false;
				
				  },


					    alltixian:function () {
							  this.money = this.allmoney;
					 
					    },
		},
	}
</script>

<style>
	/* pages/tixian/tixian.wxss */
	page {
	  background-color: rgb(245, 245, 245);
	}
	.contentView{
	  margin-left: 30rpx;
	  margin-top: 30rpx;
	  margin-right: 30rpx;
	  border-radius: 20rpx;
	  background-color: #fff;
	}
	.contentView .codeView{
	  padding: 20rpx 30rpx;
	  background-color: rgb(255, 255, 255);
	  display: flex;
	  height: 150rpx;
	  flex-direction: row;
	  align-items: center;
	}
	.codeView .title{
	  width: 70%;
	  font-size: 24rpx;
	  color: #333;
	}
	.codeView .imageView{
	  width: 30%;
	  height: 100%;
	}
	.imageView image{
	  width: 100%;
	  height: 100%;
	}
	.contentView .jifenView{
	  padding: 20rpx 30rpx;
	}
	.jifenView input{
	  margin-top: 50rpx;
	  padding: 0rpx 0rpx 0rpx 0rpx;
	  border-bottom:1px  #ededed solid;
	  font-size: 40rpx;
	  font-weight: bold;
	  height: 100rpx;
	}
	.jifenView .tishiView{
	  margin-top: 30rpx;
	  font-size: 24rpx;
	  color: #999999;
	  display: flex;
	  flex-direction: row;
	}
	.btnView{
	  margin-top: 50rpx;
	  margin-bottom: 60prx;
	  height: 160rpx;
	  padding: 0rpx 20rpx;
	}
	.btnView button{
	  background-color: #f65535;
	  border-radius: 30rpx;
	  height:100rpx;
	  font-size: 30rpx;
	  color: white;
	  line-height:100rpx;
	}
	.inText{
	margin-top: 30rpx;
	margin-left: 30rpx;
	}
	
	.mimaBg{
	  top: 0;
	  display: flex;
	  position: absolute;
	  background-color: rgb(0, 0, 0,0.7);
	  height: 100%;
	  width: 100%;
	  z-index: 999;
	}
	.mimaBg .iputView{
	  margin: 150rpx  50rpx 0rpx 50rpx;
	  height: 450rpx;
	  border-radius: 40rpx;
	  width: 100%;
	  background-color: white;
	  padding: 40rpx 0rpx;
	}
	.iputView .titleView{
	  color: #000;
	  text-align: center;
	  width: 100%;
	  font-size: 32rpx;
	}
	.iputView .itemWrap {
	  margin-top: 40rpx;
	  display: flex;
	  justify-content: space-between;
	  margin-left: 8%;
	  width: 84%;
	  color: #666;
	}
	.itemWrap .item {
	  background-color: #ededed;
	  width: 80rpx;
	  height: 100rpx;
	  display: flex;
	  justify-content: center;
	  align-items: center;
	  border-radius: 10rpx;
	}
	.item text {
	  width: 30rpx;
	  height: 30rpx;
	  background-color: #3f3c3c;
	  border-radius: 20rpx;
	}
	.iputView .input_contr {
	  left: 0;
    border:1px red solid;
	  margin-left: -9999rpx;
	}
	.iputView .btn1{
	  margin-left: 50rpx;
	  margin-right: 50rpx;
	  background-color: #f65535;
	  border-radius: 40rpx;
	  height: 80rpx;
	  margin-top: 50rpx;
	  line-height: 80rpx;
	  text-align: center;
	  color: white;
	}
	
	.iputView .btn2{
	  margin-left: 50rpx;
	  margin-right: 50rpx;
	  height: 80rpx;
	  margin-top: 0rpx;
	  line-height: 80rpx;
	  text-align: center;
	  color: #f65535;
	}

</style>
