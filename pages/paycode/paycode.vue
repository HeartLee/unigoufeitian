<template>
	<view style="width: 100%;">
		<view style="margin-top: 100rpx;">
				<!-- <view class="contentView" v-if= "currentTab ==0">
		        <image  @click="headimage" :src= "weixinImage" style="width:calc(100% - 80rpx);height:550rpx;margin-left:40rpx;margin-top:40rpx;background-color: #D2D1E2" mode="aspecFill" ></image>
		        <view class="contentViewTitle">我的微信收款二维码</view>
		        <view class="status">
		          <button @click="headimage">上传</button>
		        </view>
		    </view> -->
		     <!-- <view class="contentView"  v-if="currentTab !=0"> -->
			 <view class="contentView">
		        
		        <image  @click="headimage" :src= "zhifubaoImage" style="width:calc(100% - 80rpx);height:550rpx;margin-left:40rpx;margin-top:40rpx;background-color: #D2D1E2" mode="aspecFill" ></image>
		        <view class="contentViewTitle">我的支付宝收款二维码</view>
		        <view class="status">
		          <button @click="headimage">上传</button>
		        </view>
		    </view>
		</view>
		<view class="navbar-box">
			<block v-for="(item , index) in recordMain" :key="index">
			  <view class="nav-item" data-current="item.title"  @click="switchNav(index)">
				  <text  :class="{'active':currentTab == index}">{{item.title}}</text>
			  </view>
			</block>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				weixinImage:'',
				   zhifubaoImage:'',
				  recordMain: [
				   // {
				   // title: "微信收款"
				   // },
				   {
				   title: "支付宝收款"
				   }
				  ],
				  currentTab: 0,
				  navScrollLeft: 0,
				  winWidth: 0,
				  winHeight: 0,
				
			}
		},
		onLoad(option) {
			var that = this;
			 that.getPersoninfo();
			
		},
		methods:{
			switchNav:function(e) {
				var _this = this;
				if(_this.currentTab == e){
					return;
				}
				_this.currentTab =e;
				  
			},
			 headimage:function() {
			    var that = this;
			    uni.chooseImage({
			      count:1,
			      sizeType:['original','compressed'],
			      sourceType:['album','camera'],
			      success:function(res){
			        if(that.currentTab==0){
			          let item =  res.tempFilePaths[0];
							uni.uploadFile({
							  url: 'https://srxadmin.goufeitian.com/index.php/Api/Index/imagesupload', //服务器地址
							  fileType: "image", //ZFB必填,不然报错
							  filePath: item, //这个就是我们上面拍照返回或者先中照片返回的数组
							  name: 'file',
							  success: (uploadFileRes) => {
								that.zhifubaoImage = JSON.parse(uploadFileRes.data).data;
								// that.weixinImage = JSON.parse(uploadFileRes.data).data;
								that.uploaduserinfo(that);
							  }
							});
			      }else{
					  let item =  res.tempFilePaths[0];
					  
						uni.uploadFile({
						  url: 'https://srxadmin.goufeitian.com/index.php/Api/Index/imagesupload', //服务器地址
						  fileType: "image", //ZFB必填,不然报错
						  filePath: item, //这个就是我们上面拍照返回或者先中照片返回的数组
						  name: 'file',
						  success: (uploadFileRes) => {
							that.zhifubaoImage = JSON.parse(uploadFileRes.data).data;
							that.uploaduserinfo(that);
						  }
						});
			      }
			      }
			    })
			},
			uploaduserinfo:function(that) {
			  var data;
			  if(that.currentTab == 0){
			    data = {
			      openid:localStorage.getItem("opendid"),
			      zhifubao_url:that.zhifubaoImage
			    }
			  }else{
			    data = {
					openid:localStorage.getItem("opendid"),
					weixin_url:that.weixinImage
			    }
			  }
			    wx.request({
			       url: 'https://srxadmin.goufeitian.com/index.php/Api/Index/updatePay',
			      data: data,
			      success: function (res) {
			         that.getPersoninfo();
			       },
			    })
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
              that.weixinImage = res.data.data.weixin_url;
              that.zhifubaoImage = res.data.data.zhifubao_url;
            }
          },
        })
      },
		}
	}
</script>

<style>
	/* pages/paycode/paycode.wxss */::-webkit-scrollbar {
	  width: 0;
	  height: 0;
	  color: transparent;
	 }
	  
	 .navbar-box {
		width: 100%;
	  height: 70rpx;
	  line-height: 70rpx;
	  position: fixed;
	  top: 0rpx;
	  background: white
	 }
	  
	 .nav-item {
	  display: inline-block;
	  width: 25%;
	  text-align: center;
	  color: #666;
	 }
	  
	 .nav-item text {
	  padding-bottom: 10rpx;
	 }
	   
	 page {
	  /* background: #f2f2f2; */
	  font-size: 28rpx;
	 }
	  
	 .active {
	  color: #f65535;
	  border-bottom: 4rpx solid #f65535;
	  font-size: 28rpx;
	  box-sizing: border-box;
	 }
	  
	 .menu {
	  font-size: 14rpx;
	  width: 100%;
	  /* overflow-x: scroll; */
	  /* border-bottom: 20rpx solid #f2f2f2; */
	  padding: 30rpx 30rpx 0rpx 30rpx;
	  box-sizing: border-box;
	  display: flex;
	  justify-content: space-between;
	  position: fixed;
	  top: 0rpx;
	  z-index: 999;
	  background: white;
	 }
	  
	 .chooseNav {
	  padding-bottom: 10rpx;
	 }
	  
	 .contentView{
	   margin-top: 50rpx;
	   margin-left: 30rpx;
	   width: calc(100% - 60rpx);
	   height: 850rpx;
	   border-radius: 50rpx;
	  box-shadow:0px 2px 5px 5px #ccc;
	 }
	 .contentViewTitle{
	   width: 100%;
	   margin-right: 0rpx;
	   margin-top: 30rpx;
	   text-align: center;
	   color: #333;
	   font-size: 32rpx;
	 }
	 .status button {
	  display: inline-block;
	  background: #f65535;
	  border-radius: 50rpx;
	  font-size: 36rpx;
	  margin-left: 20rpx;
	  color: white;
	  margin-left: 30rpx;
	  width: calc(100% - 60rpx);
	  box-sizing: border-box;
	  height: 100rpx;
	line-height: 100rpx;
	  margin-top: 30rpx;
	 }
	 
</style>
