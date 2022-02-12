<template>
	<view  class="listcat">
	    <view class="bg-white statsinfo">
	        <view class='shopname'>订单状态</view>

	        <view class="cu-steps">	
				<view class="cu-item" :class="index>basics?'':'text-red'" v-for="(item,index) in basicsList" :key="index">
					<text :class="index>basics?'cuIcon-title':'cuIcon-' + item.icon"></text> {{item.name}}
				</view>
	        </view>
	
	        <view class="qiangdan">{{orderInfo.createtime}}抢单成功，放弃次数超过3次，关一天小黑屋</view>
	         <view class="qiangdan" v-if="orderInfo.quxiaotime!=null">{{orderInfo.createtime}}  付款后取消订单！</view>
	        <view class="qiangdan"  v-if="orderInfo.ziliaotime!=null">{{orderInfo.ziliaotime}}  资料上传成功</view>
	        <view class="qiangdan"  v-if="orderInfo.status=='3' && orderInfo.reason!=''">{{orderInfo.shenhetime}}  到账成功<text decode>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</text>备注：{{orderInfo.reason}}</view>
	        <view class="qiangdan"  v-if="orderInfo.status=='3' && orderInfo.reason!=''">{{orderInfo.shenhetime}}  到账成功  </view>
	        <view class="qiangdan"  v-if="orderInfo.status=='9'">订单已失效 </view>
	      </view>  
	
	
	      <view class="paybody">
	        <view>
	              <view class="paybodyleft">
	                <image  mode='widthFix' :src="orderInfo.shophead"></image>
	              </view>
	              <view class="paybodyright">
	                <view class="title">{{orderInfo.shopname}}</view>
	                <view class="desc">{{orderInfo.descc}}</view>
	                <view class="price">¥{{orderInfo.money}}</view>
	              </view>
	          </view>
	          <view class="tiaozhuan">
	              <view class="title"  @click="toggleMjd">跳转到外卖平台</view>
	          </view>
	      
	          <view class="payinfo">
	              <view class="titl">支付信息</view>
	              <view class="dss">
	                 支付：
	                 <span class="yanse">¥{{orderInfo.money}}</span>
	                 <span class="fanli"> 返利</span>
	                  <span class="yanse" style="margin-left: 20rpx;" v-if="orderInfo.vipflag == 1">¥{{orderInfo.vipfanli}} （0.5为活动报名费，订单审核通过后后扣费）</span>
	                  <span class="yanse" style="margin-left: 20rpx;" v-else>¥{{orderInfo.fanli}} （0.5为活动报名费，订单审核通过后后扣费）</span>
	              </view>
				 
				  <view v-if="orderInfo.reason != null"  style="color:red;margin-top: 10px;margin-bottom: 10px;">订单驳回原因：{{orderInfo.reason}}</view>
				  
				  
	          </view>       
	      </view>
	      <view class="paybodyxinxi">
	            <view>
	              <view class="paybodydata">
	                <view class="information">订单信息</view>
	                <view class="order" style="margin-top: 20rpx;">订单号：{{orderInfo.ordersn}}</view>
	                <view class="order" v-if="orderInfo.mobile == null">手机号：</view>
	                <view class="order" v-else>手机号：{{orderInfo.mobile}}</view>
	                <view class="order" v-if="orderInfo.mobile == null">外卖平台下单订单号：</view>
	                  <view class="order" v-else>外卖平台下单订单号：{{orderInfo.outourderson}}</view>
	                
					<view class="datum">资料详情</view>
	                <view class="shiliView" >
						
	                 <block v-for="(item,index) in tabContent" :key="index">
	                    <image :src="item"  :style="{height:Imageheight}"></image>
	                </block>
	                </view>
	              </view>
	            </view>
	          </view>
	         <view class="btnView">
	         <button v-if="orderInfo.status== 1"  @click="updata(orderInfo.id)"> 上传资料</button>
	         </view>
			 
			 
			 <view class="mimaBg"  v-if="isshowmjd" catchtouchmove='true'>
			   <view class="mimaBgcontentView">
			     <view class="image1">
			       <image :src="orderInfo.qrcode" style="width: 100%;" mode="aspectFit"></image>
			     </view>
			     <view class="title1">
			       <view>扫码进入平台</view>
			     </view>
			     <view class="buttonView" @click="toggleMjd">
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
			 Imageheight:0,
			    orderState : '1',
			    orderInfo:{},
				isshowmjd:false,
			    basicsList: [{
			          icon: 'radioboxfill',
			          name: '抢单'
			        }, {
			          icon: 'radioboxfill',
			          name: '待上传'
			        }, {
			          icon: 'radioboxfill',
			          name: '审核'
			        }, {
			          icon: 'radioboxfill',
			          name: '完成'
			        }],
			        basics:0,
			        orderid:'',
			        tabContent:[],
			        isvip:'',
					fanli:'',
		}
	},
	onLoad(options) {
		var that = this; 
		let systemInfo = uni.getSystemInfoSync()
		let pxToRpxScale = 750 / systemInfo.windowWidth;
		this.Imageheight = (systemInfo.windowWidth*pxToRpxScale-90)/3;
		this.orderid = options.orderid;
		this.isvip = '',  		  
		this.getorderinfo();
		
	},
	methods:{
		 getorderinfo:function (options) {
		    var that = this;
		    uni.request({
		      url: 'https://srxadmin.goufeitian.com/index.php/Api/Index/getOrdernfo',
		      data: {
		        orderid: that.orderid
		      },
		      success:function (res) {
		        let int_num = parseInt(res.data.data.status);
		        var a = res.data.data.file;
		      if(res.data.data.status=='9'){
		      	that.basicsList = [{
		            icon: 'radioboxfill',
		            name: '抢单'
		          }, {
		            icon: 'radioboxfill',
		            name: '待上传'
		          }, {
		            icon: 'radioboxfill',
		            name: '审核'
		          }, {
		            icon: 'radioboxfill',
		            name: '已过期'
		          }]
		      }
		      if(res.data.data.status=='4'){
		      	that.basicsList = [{
		            icon: 'radioboxfill',
		            name: '抢单'
		          },{
		            icon: 'radioboxfill',
		            name: '待上传'
		          }, {
		            icon: 'radioboxfill',
		            name: '已取消'
		          }]
		      }
		        if(a==''){
					that.orderInfo = res.data.data;
					that.basics = int_num;
		        
		        }else{
					that.orderInfo = res.data.data;
					that.tabContent = a.split(",");
					that.basics = int_num;
		      }
			 
		      }
		    })
		 },
		 toggleMjd(){
		 if(this.isshowmjd == false){
		 	 this.isshowmjd = true
		    }else{
		 	 this.isshowmjd = false
		    }
		 },
		 updata(e){
			 uni.navigateTo({
			   url: '../../pages/updatum/updatum?orderid='+e,
			 })
		 },
		 
	},
}
</script>

<style>
	::-webkit-scrollbar {
	  width: 0;
	  height: 0;
	  color: transparent;
	 }
	
	.listcat {
	  background:#ededed;
	}
	
	.statsinfo{
	  padding:30rpx 40rpx;
	}
	
	.statsinfo .shopname{
	  font-size: 32rpx;
	  margin-bottom: 30rpx;
	  color: #000;
	  font-weight: bold;
	}
	.statsinfo .qiangdan{
	  margin-top:40rpx;
	  font-size: 24rpx;
	}
	
	.paybody{
	    background: #ffffff;
	    margin-top: 30rpx;
	    
	    padding:40rpx 30rpx  20rpx 30rpx;
	}
	.paybody .paybodyleft{
	  display: inline-block;
	  width: 30%;
	
	}
	.paybodyleft image{
	  width: 100%;
	  border-radius: 20rpx;
	}
	.shiliView{
	  width: 100%;
	}
	.shiliView image{
	  margin-top: 20rpx;
	  width: 30%;
	  height: 100%;
	  /* height: width/3; */
	  margin-right: 3%;
	  border-radius: 5%;
	}
	.paybody .paybodyright{
	  padding-left:4%;
	  display: inline-block;
	  width: 65%;
	}
	.paybody .title{
	  font-weight: bold;
	}
	
	.paybody .desc{
	  margin-top: 20rpx;
	  font-size: 24rpx;
	  color:#ff9933;
	}
	.paybody .price{
	  margin-top: 20rpx;
	  color:#ff9933;
	  font-weight: bold;
	}
	.tiaozhuan{
	  text-align: right;
	  margin-top:30rpx;
	  padding-bottom: 20rpx;
	  border-bottom:1px  #ededed solid;
	}
	.tiaozhuan .title{
	   color:#ff9933;
	   font-size: 26rpx;
	}
	
	
	
	.paybodyxinxi{
	  /* background: #ffffff; */
	  margin-top: 20rpx;
	  padding:30rpx 30rpx;
	}
	.paybodyxinxi .information{
	  font-weight: bold;
	}
	.paybodyxinxi .order{
	  margin-top: 10rpx;
	  font-size: 24rpx;
	  color: #666;
	}
	.paybodyxinxi .datum{
	  font-weight: bold;
	  margin-top: 30rpx;
	}
	
	
	.payinfo{
	  margin-top: 30rpx;
	}
	.payinfo .titl{
	  font-weight: bold;
	  font-size: 30rpx;
	}
	.payinfo .yanse{color:#ff9933}
	.payinfo .fanli{
	  margin-left: 40rpx;
	}
	.payinfo .dss{
	  margin-top: 20rpx;
	}
	.btnView{
	  margin-top: 20rpx;
	  padding: 0rpx 30rpx;
	  height: 100rpx;
	  display: flex;
	  justify-content: center;
	  align-items: center;
	}
	.btnView button{
	  width: 100%;
	  background-color: #f65535;
	  color: white;
	  height: 100%;
	  border-radius: 50rpx;
	  font-size: 32rpx;
	  line-height: 100rpx;
	  margin-bottom: 30rpx;
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
	  width:  50rpx;
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
	.mimaBgcontentView{
	  margin-left: 50rpx;
	  width: calc(100% - 100rpx);
	  border-radius: 30rpx;
	  background-color: #fff;
	}
	.mimaBgcontentView .image1{
			 margin-top: 30rpx;
			 margin-left: 30rpx;
			 margin-right: 30rpx;
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
