<template>
	<view  style="width: 100%;">
	<view class="shopHead">
	  <image :src="shopInfo.head" mode="widthFix"></image>
	  <view class="shopview">
	    <view style="font-weight: bold;">{{shopInfo.title}}
		</view>
	    <view class="addressView">
	      <image src="../../static/icondingwei.png" mode="widthFix"></image>
	      <view class="address">{{shopInfo.address}}</view>
	    </view>
	    <view class="juli">
	      <view class="title">距离{{shopInfo.distance}}Km</view>
	      <view class="tiaozhuang" v-if="shopInfo.shopid !='' && shopInfo.shopidelm !=''">
	       <image src="../../static/meituan.png" mode="widthFix" style="margin-right: 10rpx;"></image>
	       <image src="../../static/elment.png" mode="widthFix" ></image>
	       <view class="text" @click="copyname(shopInfo.title)">复制店名</view>    
	      </view>
	       <view class="tiaozhuang" v-else-if="shopInfo.shopid !=''">
	       <image src="../../static/meituan.png" mode="widthFix"></image>
	       <view class="text" @click="copyname(shopInfo.title)">复制店名</view>    
	      </view>
	       <view class="tiaozhuang" v-else="shopInfo.shopidelm !=''">
	       <image src="../../static/elment.png" mode="widthFix"></image>
	       <view class="text" @click="copyname(shopInfo.title)">复制店名</view>    
	      </view>
	    </view>
	  </view>
	</view>
	<view class="miaoshu" style="color:#ff3000">
	    {{shopInfo.dess}}
	</view>
	<view class="line"></view>
	<view class="listBox">
	   <block v-for="(item , index) in shopInfo.discont" :key="index">
	       <view class="listView">
	          <view class="leftView">
	            <view class="titleView">
	               <image src="../../static/elment.png" v-if="item.type=='饿了么'" mode="widthFix"></image>
	                  <image src="../../static/meituan.png"  v-if="item.type=='美团'" mode="widthFix"></image>
	
	                   <view  v-if ="item.type=='美团'">
	                       <view class="title"  style="color: #FBC715;margin-top: -10rpx;" >会员满{{item.cost}}奖励{{item.vipfanli}}积分</view>
	                   <view class="title"   style="color: #FBC715;margin-top: 5rpx;"   >非会员满{{item.cost}}奖励{{item.fanli}}积分</view>
	                  </view>
	                 <view  v-if="item.type=='饿了么'">
	                        <view class="title"   style="color: #3399ff;margin-top: -10rpx;" >会员满{{item.cost}}奖励{{item.vipfanli}}积分</view>
	                   <view class="title"   style="color: #3399ff;margin-top: 5rpx;" >非会员满{{item.cost}}奖励{{item.fanli}}积分</view>
	                   </view>
	            </view>
	              <view class="infomiaoshu">{{item.descc}}
	              </view>
	              <view class="time">抢购时间：{{item.iswork}}</view>
	          </view>
	          <view class="rightView">
	              <image src="../../static/qianggou.png" mode="widthFix" @click="submitinfo(item)"></image>
	              <view class="title">当前剩余：{{item.remain_num}}</view>
	          </view>
	          
	       </view>
	   </block>
	   </view>
	   <view class="shopdetail" style="margin-top:20rpx;width: 100%;">
	     <rich-text :nodes="content" ></rich-text>
	   </view>
  <view class="mimaBg" v-if="isshowCode" catchtouchmove='true'>
       <view class="contentView">
        <view style="width:100%"> 
            <image src="../../static/shanchu.png" style="width:60rpx;margin-left:calc(100% - 110rpx);" mode="widthFix" @click="dismisscode"></image>
            <view class="contentViewTitle">复制店名</view>
        </view>
        <view class="waimaiView" v-if="shopInfo.shopid !='' && shopInfo.shopidelm !=''" >
           <image src="../../static/meituan.png" mode="widthFix" style="margin-right: 10rpx;"></image>
           <view class="text" @click="meituan">复制店名</view>
      </view>

       <view class="waimaiView" v-if="shopInfo.shopid !='' && shopInfo.shopidelm !=''" style=" margin-top:40rpx;margin-bottom:60rpx">
           <image src="../../static/elment.png" mode="widthFix" style="margin-right: 10rpx;"></image>
           <view class="text" @click="elment">复制店名</view>
      </view>


       <view class="waimaiView" v-else-if="shopInfo.shopid !=''" style="margin-bottom:60rpx">
           <image src="../../static/meituan.png" mode="widthFix" style="margin-right: 10rpx;"></image>
           <view class="text" @click="meituan">复制店名</view>
      </view>
       <view class="waimaiView" v-else-if="shopInfo.shopidelm !=''"  style="margin-bottom:60rpx">
           <image src="../../static/elment.png" mode="widthFix" style="margin-right: 10rpx;"></image>
           <view class="text" @click="elment">复制店名</view>
      </view>
	  
	  <view class="waimaiView"  style="margin-bottom:60rpx">
	    
	       <view class="text" @click="copyname(shopInfo.title)" >复制店名</view>
	  </view>
	  
	  
      </view>
  </view>
  
  <view class="mimaBg"  v-if="isshowCode1" catchtouchmove='true'>
      <view class="mimaBgcontentView">
  		
  			<image class="image1" :src="waimaiCode" style="width: 100%;" mode="aspectFit"></image>
  	
  		<view class="title1">
  				等待5秒钟，长按二维码图片可调用微信小程序
  		</view>
		<view class="title1">
				若未显示二维码，请复制店名在外卖平台搜索下单即可
		</view>
  		<view class="buttonView" @click="showCode1">
  				我知道了
  		</view>
  	</view>
  </view>		
</view>

</template>

<script>
	export default {
		data() {
			return {
				storeid:'',
				lat:'',
				long:'',
				isvip:'',
				shopInfo:{},
        qrcode_meituan:'',
        qucode_elm:'',
				goodInfo:{},
				content:'',
				usernickname:null,
				isshowCode:false,
				isshowCode1:false,
				waimaiCode:'',
			}
		},
		onLoad(options) {
		
		  let  opion = JSON.parse(options.opion)
		  var that = this;
			that.storeid = opion.storeid;
			that.lat = opion.lat;
			that.long = opion.long;
			that.isvip = opion.isvip;
		  this.getshopByid();
			
		},
		methods:{
		 getshopByid:function(){
		    var _this = this;
		    wx.request({
		      url: 'https://srxadmin.goufeitian.com/index.php/Api/Index/getshopByid',
		      data: {
		        lng:_this.long,
		        lat:_this.lat,
		        shopid:_this.storeid,
		      },
		      success:function (res) {
		        var content = res.data.data;
		        _this.shopInfo=content;
		        _this.qrcode_meituan = content.qrcode_meituan;
		        _this.qrcode_elm = content.qucode_elm;
		        _this.content = content.content.replace(/\<img/g, "<img style='width: 100%;'")
		      }
		    })
		  },
		  tiaozhuanAPP(){
			  this.isshowCode = true;
		  },
		   dismisscode:function(){
			   this.isshowCode = false;
		   
		 },
		 meituan:function(){
			this.isshowCode = false;
			this.waimaiCode = this.qrcode_meituan;
		   this.isshowCode1 = true;
		 },
		 showCode1(){
			
		   this.isshowCode1 = false;
		 
		 },
		 copyname(name){
			 uni.setClipboardData({
				data: name,
				success: () => { //复制成功的回调函数
					uni.showToast({ //提示
						title: '复制成功'
					})
				}
			});
			 
		 },
		 elment:function(){
			this.isshowCode = false;
			this.waimaiCode = this.qrcode_elm
			 this.isshowCode1 = true;
		 },
		  submitinfo:function(item){
		    let thatinfo = this;
		  	this.goodinfo = item;
		  	if(localStorage.getItem("opendid")=='' || localStorage.getItem("opendid")==null){
		  		uni.showModal({
		  		   title: '提示',
		  		   content: '您还没登录，请先登录',
		  		   confirmText: '去登录',
		  		   confirmColor: '#f65535',
		  		   success (res) {
		  		    uni.navigateTo({
		  		    	url:'../login/login'
		  		    })
		  		 }
		  		 })
		  		return;
		  	}	
		  	if(item.daoqi == '0'){
		  	    var string = '';
		  	    if(item.start_date){
		  	      string =  item.start_date.substring(5,10);
		  	       string = string.replace('-', '.');
		  	    };
		  		debugger
		  	    uni.showModal({
		  	      title: '提示',
		  	      content: ' 预售订单,活动时间'+ string+'  '+item.isdiscont_start + '开始',
		  	      confirmText: '确认',
		  	      confirmColor: '#f65535',
		  	      success (res) {
		  	    }
		  	    })
		  	   return;
		  	  }
		  	  if(!item.isdiscont){
		  	    
		  	      uni.showModal({
		  	        title: '提示',
		  	        content: ' 预售订单,活动时间'+item.iswork + '开始',
		  	        confirmText: '确认',
		  	        confirmColor: '#f65535',
		  	        success (res) {
		  	         
		  	              }
		  	     })
		  	     return;
		  	    }
		  		
		  		 uni.showModal({
		  		    title: '提示',
		  		    content: '已明确本店铺活动内容及说明，参与抢单后次日早上9 点以前（凌晨夜宵订单，当日早9 点前）必须完成【订单信息】截图并上传至惠食天下霸王餐小程序，否则视为品鉴任务失败，您确认抢单？',
		  		    confirmText: '确认',
		  		    confirmColor: '#f65535',
		  		    success (res) {
		  		      if (res.confirm) {
		  		        uni.request({
		  		          url: 'https://srxadmin.goufeitian.com/index.php/Api/Index/submitinfo',
		  		          data: {
		  		            openid: localStorage.getItem("opendid"),
		  		            discontid:item.id,
		  		          },
		  		          
		  		          success:function (res) {
		  		            var content = res.data.data;
		  		            console.log(content);
		  		            if(res.data.status=='1'){
								uni.showToast({
								      icon:'none',
								      title:res.data.info
								    })
								// if(item.type == '美团'){
								//   thatinfo.isshowCode1 = true;
								//   thatinfo.waimaiCode = thatinfo.qrcode_meituan;
								// }else{
								//   this.isshowCode1 = true;
								//   thatinfo.waimaiCode = thatinfo.qrcode_elm;
								// }
		  		            // _this.rocessPay(JSON.parse(content))
		  		          }else{
                        uni.showToast({
		  		              icon:'none',
		  		              title:res.data.info
		  		            })
		  		          }
		  		          }
		  		        })
		  		      }
		  		    }
		  		  })
		  }
			
		}
	}
</script>

<style>
	
	/* pages/shopInfo/shopInfo.wxss */
	page{
	  background:#fff;
	}
	.shopHead{
	  padding: 30rpx 30rpx;
	  display: flex;
	  flex-direction: row;
	}
	.shopHead image{
	  width: 30%;
	  border-radius: 10rpx;
	}
	.shopHead .shopview{
	  font-size: 28rpx;
	
	  color: #333;
	  width: 66%;
	  padding-left: 20rpx;
	}
	.shopview .addressView{
	  display: flex;
	  flex-direction: row;
	  margin-top: 20rpx;
	}
	.addressView image{
	  width: 40rpx;
	}
	.addressView .address{
	  font-size: 22rpx;
	  color: #999;
	}
	.shopview .juli{
	  margin-top: 25rpx;
	  font-size: 22rpx;
	  display: flex;
	  flex-direction: row;
	}
	.juli .title{
	  font-size: 22rpx;
	  color: #999;
	}
	.juli .tiaozhuang{
	  right: 0;
	  position: absolute;
	  margin-right: 30rpx;
	  display: flex;
	  flex-direction: row;
	  align-items: center;
	}
	.tiaozhuang .text{
	  font-size: 22rpx;
	  margin-left: 10rpx;
	  color:#f65535 ;
	  border: 3rpx solid #f65535;
	  padding: 5rpx 20rpx;
	  border-radius: 100rpx;
	}
	.tiaozhuang image{
	  width:30rpx;
	}
	
	.miaoshu{
	  margin-top: 20rpx;
	  margin-left: 30rpx;
	  margin-right: 30rpx;
	  border-radius: 20rpx;
	  background-color:rgb(240, 242, 247);
	  font-size: 24rpx;
	  color: #999;
	  padding: 10rpx 20rpx 10rpx  10rpx;
	}
	.line{
	  margin-top: 40rpx;
	  width: 100%;
	  height: 2rpx;
	  background-color: #ededed;
	}
	.listBox{
	  padding: 0rpx 30rpx 0rpx 30rpx;
	  
	}
	.listBox .listView{
	  margin-top: 30rpx;
	  border-radius:25rpx;
	  /* margin-left: 30rpx; */
	  box-shadow:0px 3px 3px 3px #e1e1e1;
	  padding: 30rpx 0rpx 30rpx 30rpx;
	  width: 100%;
	  flex-direction: row;
	  display: flex;
	}
	.listView .leftView{
	  width: 70%;
	}
	.leftView .titleView{
	  flex-direction: row;
	  display: flex;
	
	}
	.titleView image{
	  width: 40rpx;
	}
	.titleView .title{
	  color: #f65535;
	  font-size: 22rpx;
	  margin-left: 20rpx;
	  
	}
	.leftView .infomiaoshu{
	  margin-top: 10rpx;
	  margin-left: 60rpx;
	  color: #333;
	  font-weight: bold;
	  font-size: 22rpx;
	}
	.leftView .time{
	  margin-top: 10rpx;
	  margin-left: 60rpx;
	  color: #999;
	  font-size: 22rpx;
	}
	.listView .rightView{
	  width: 30%;
	  padding: 10rpx 0rpx 0rpx 0rpx;
	}
	.rightView image{
	  margin-left: 20%;
	   width: 60%;
	 
	}
	.rightView .title{
	  margin-top: 20rpx;
	  color: #999;
	  text-align: center;
	  width: 100%;
	  font-size: 22rpx;
	}
	
	.dianming{
	  top: 0;
	  right: 0;
	  position: fixed;
	  box-shadow:0px 0px 4px 4px #cccccc;
	  border-bottom-left-radius: 50rpx;
	  border-top-left-radius: 50rpx;
	  margin-right: 0rpx;
	  padding: 10rpx 20rpx 5rpx  30rpx;
	  background-color: #ffffff;
	}
	.dianming .text{
	  font-size: 23rpx;
	  color: #666;
	  margin-top: -5rpx;
	}
	.homepage{
	  top: 0;
	  right: 0;
	  position: fixed;
	  box-shadow:0px 0px 5px 5px #eeeeee;
	  border-bottom-left-radius: 50rpx;
	  border-top-left-radius: 50rpx;
	  margin-right: 0rpx;
	  padding: 10rpx 20rpx 5rpx 20rpx;
	  font-size: 23rpx;
	  color: #666;
	  margin-top: 120rpx;
	  background-color: #f65535 ;
	}
	.homepage image{
	  width: 50rpx;
	}
	
	.share{
	  top: 0;
	  right: 0;
	  position: fixed;
	  box-shadow:0px 0px 5px 5px #eeeeee;
	  border-bottom-left-radius: 50rpx;
	  border-top-left-radius: 50rpx;
	  margin-right: 0rpx;
	  padding: 10rpx 20rpx 5rpx 20rpx;
	  font-size: 23rpx;
	  color: #666;
	  margin-top: 240rpx;
	  background-color: #f65535 ;
	}
	.share image{
	  width: 50rpx;
	}
	
	.shopdetail {
	  padding: 30rpx 30rpx 30rpx 30rpx;
	}
	
	button::after{
	
	  border: 0;
	  
	}
	
	.mimaBg{
	  top: 0;
	  display: flex;
	  position: fixed;
	  background-color: rgb(0, 0, 0,0.7);
	  height: 100%;
	  width: 100%;
	  z-index: 999;
	  align-items: center;
	}
	.contentView{
	  margin-left: 80rpx;
	  width: calc(100% - 160rpx);
	  border-radius: 50rpx;
	  background-color: #fff;
	}
	.contentViewTitle{
	  width: calc(100% - 100rpx);
	  margin-right: 0rpx;
	  margin-top: 0rpx;
	  margin-left: 30rpx;
	  text-align: center;
	  color: #000;
	  font-size: 28rpx;
	}
	.contentView .titleview{
	margin-top: 20rpx;
	  display: flex;
	width: calc(100% - 100rpx);
	  flex-direction: row;
	}
	.contentView .waimaiView{
	  margin-top: 60rpx;
	  display: flex;
	  flex-direction: row;
	  align-items: center;
	  vertical-align: center; /**垂直居中*/
	  justify-content: center; 
	}
	.waimaiView image{
	  width: 40rpx;
	}
	.waimaiView .text{
	  font-size: 22rpx;
	  margin-left: 0rpx;
	  color:#f65535 ;
	  border: 3rpx solid #f65535;
	  padding: 5rpx 20rpx;
	  border-radius: 100rpx;
	}
	.mimaBgcontentView{
	  margin-left: 50rpx;
	  width: calc(100% - 100rpx);
	  border-radius: 30rpx;
	  background-color: #fff;
	}
	.mimaBgcontentView .image1{
		margin-top: 30rpx;

	}
	.mimaBgcontentView .title1{
			font-size: 24rpx;
			margin-top: 30rpx;
			width: 100%;
			text-align: center;
	}
	.mimaBgcontentView .buttonView{
		font-size: 24rpx;
		margin-top: 30rpx;
		margin-left: 80rpx;
			margin-right: 80rpx;
			border: 2rpx solid #666666;
			border-radius: 25rpx;
			background-color: #F0F0F0;
			height: 50rpx;
			color: #666666;
			margin-bottom: 30rpx;	
			line-height:50rpx;
		text-align: center;
	}
</style>
