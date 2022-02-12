<template>
	<view>
	<view class="contentView">
	  <view class="listView">
	      <view class="title">订单号</view>
	      <input v-model="orderInfo.ordersn" disabled></input>
	  </view>
	  <!-- <view class="listView">
	        <view class="title">金额</view>
	        <input v-model="orderInfo.money"  style="color:#f65535;font-size:32rpx" disabled></input>
	  </view> -->
	  <view class="listView">
	         <view class="title">实际支付金额</view>
	         <input  placeholder="请输入金额"  @input="getPayMoney"></input>
	  </view>
	  <view class="listView">
	         <view class="title">外卖平台订单号</view>
	         <input placeholder="请输入订单号"  @input="getPayorder"></input>
	  </view>
	</view>
	<view class="infoLabel">* 请按要求上传资料(订单截图+好评截图)。</view>
	<view class="infoLabel" style=" margin-top:10rpx">* 正常图片数量为1-4张,安卓手机建议截长图</view>
	
	<view class="shiliLabel">图片示例</view>
		<view class="shiliView" :style="{'height':Imageheight}">
		<block v-for="(item,index) in answerImg" :key="index">
		 <image @click="topic_preview(item.id,item)" :src="item"></image>
		 </block>
	</view>
	<view class="shiliLabel"  style=" margin-top:0rpx">选择图片上传</view>
	<view style="width:100%;float:left;margin-top:25rpx;margin-left:40rpx;padding-bottom:50rpx;" class="weui-uploader__bd">
	   <view class="weui-uploader__files">
		   <block v-for="(item,index) in pics" :key="index">
			   <view class="weui-uploader__file" >
				    <image src="../../static/scanCode_delete.png" v-if="item!=''"
				     @click="img_delete(index)"  class="image_view_css" :style="{'margin-left':margin_left}"></image>
				 <image class="weui-uploader__img"  v-if="item!=''" :src="item"  :style="{'height':height1,'width':height1}" mode="aspectFill">
					 
				 </image>  
			
				</view>
			</block>
		</view>
		
		   <view class="weui-uploader__input-box"  :style="{height:height1}" v-show="isShow">
			<view class="weui-uploader__input" @click="chooseImage" ></view>
		  </view>
	</view>
	
		<view class="btnView" @click="sureClick">确认提交</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
				Imageheight:0,
				margin_left:0,
				height1:0,
			    pics: [],
			    count: [1, 2, 3,4],
			    isShow:true,
			    outourderson:'',
				orderInfo:{},
			    payamount:'',
			    orderid:'',
			    imgaeurl:'',
			    pics:[],
				loadflag:false,
			    answerImg:['../../static/eg_ele.png', '../../static/eg_meituan.png'],
		}
	},
	onLoad(option) {
	   let systemInfo = uni.getSystemInfoSync()
		let pxToRpxScale = 750 / systemInfo.windowWidth;
		this.Imageheight =  (systemInfo.windowWidth*pxToRpxScale-60)/3+30;
		this.Imageheight = this.Imageheight+'rpx';
		this.margin_left = (systemInfo.windowWidth*pxToRpxScale-60)/3- 60;
		this.margin_left = this.margin_left +'rpx';
		
		this.height1 = (systemInfo.windowWidth*pxToRpxScale-60)/3- 40;
		this.height1 = this.height +'rpx';
		console.log(this.Imageheight);
		this.orderid = option.orderid;
		this.getorderinfo();
		isShow: (option.isShow == "true" ? true : false)
		
	},
	methods:{
		 getPayMoney:function(e) {
		    var that = this;
			that.payamount = e.detail.value;
		  
		  },
		  getPayorder:function(e) {
		    var that = this;
			that.outourderson = e.detail.value;
		  },
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
				that.orderInfo = res.data.data;
		    }})
		 },
		   topic_preview: function(id,item){
		   
		         var that = this;
		         var id = id;
		         var url = item;
		      
		         //通过循环在数据链里面找到和这个id相同的这一组数据，然后再取出这一组数据当中的图片
		         var data = that.answerImg;
		       
		         uni.previewImage({
		         current: url, // 当前显示图片的http链接
		         urls: data // 需要预览的图片http链接列表
		         })
		     },
			  chooseImage:function(){
			     var that = this;
			     var pics = this.pics;
			     uni.chooseImage({
			       count:4-pics.length,
			       sizeType:['original','compressed'],
			       sourceType:['album','camera'],
			       success:function(res){
			         var temfilesize = res.tempFiles[0].size;
			         var imgsrc = res.tempFilePaths;


			         for(var i = 0;i<res.tempFilePaths.length;i++){
                 let item =  res.tempFilePaths[i];
				 uni.showLoading({
				 			    title: '加载中'
				 			});
                 uni.uploadFile({
                   url: 'https://srxadmin.goufeitian.com/index.php/Api/Index/imagesupload', //服务器地址
                   fileType: "image", //ZFB必填,不然报错
                   filePath: item, //这个就是我们上面拍照返回或者先中照片返回的数组
                   name: 'file',
                   success: (uploadFileRes) => {
					   setTimeout(function () {
					   		  uni.hideLoading();
					   		}, 200);
							
                     that.pics.push(JSON.parse(uploadFileRes.data).data)

                   }
                 });
			         }
			         if(pics.length==4){
						 that.isShow = false;
			         }
			       },
			     })
			   },
			   img_delete:function(e){
			       var that = this;
			       var pics = this.pics;
			       var vvv = [];
			       for(var i = 0;i<pics.length;i++){
			         if(i == e){
			         }else{
			           vvv.push(pics[i]);
			         }
			       }
			      
			       if(pics.length==4){
					    that.isShow = false;
			       }else{
					    that.isShow = true;
			       }
				    that.pics = vvv;
			       
			     },
		sureClick:function(){
             var that = this;
            
             if(that.payamount==''){
               uni.showToast({
                 icon: "none",
                 title: '请输入金额',
               })
               return;
             }
             if(that.outourderson==''){
               uni.showToast({
                 icon: "none",
                 title: '请输入外卖订单号',
               })
               return;
             }
             if(that.pics.length==0){
               uni.showToast({
                 icon: "none",
                 title: '请选择图片',
               })
               return;
             }
             
			if(that.loadflag == true){
				 return;
			 }
			 that.loadflag = true;
			 
             uni.request({
               url: 'https://srxadmin.goufeitian.com/index.php/Api/Index/uploadOrder',
               data: {
                 orderid: that.orderid,
                 payamount:that.payamount,
                 outourderson:that.outourderson,
                 file:that.pics
               },
               method:'GET',
               success:function (res) {
                 if(res.data.status=='1') {
					that.loadflag = false; 
                   uni.showToast({
                     icon:'none',
                     title: '上传资料成功',
                   })
                   uni.redirectTo({
                     url: '../../pages/operation/operation?orderid='+that.orderid
                   })

                 }else{
                   uni.showToast({
                     icon:'none',
                     title: res.data.info,
                   })
				   that.loadflag = false;
                 }
               }
             })
				   },
		  
	},
}
</script>

<style>
	::-webkit-scrollbar{
			  display: none;
		}
	/* pages/datum/updatum.wxss */
	.contentView{
	  padding: 40rpx 40rpx;
	  background-color: #fff;
	  box-shadow:0px 2px 5px 2px #eee;
	  margin-left: 30rpx;
	  margin-right: 30rpx;
	  margin-top: 30rpx;
	  border-radius: 20rpx;
	}
	.listView{
	  height: 100rpx;
	  border-bottom: 2rpx solid #f2f2f2;
	  display: flex;
	  /*row 横向  column 列表  */
	  flex-direction: row;
	}
	.listView .title{
	  line-height: 100rpx;
	  height: 100%;
	  color: #333;
	}
	.listView input{
	  text-align: right;
	  position: absolute;
	  right: 0;
	  /* margin-left: 20rpx; */
	  height: 100rpx;
	  margin-right: 80rpx;
	  line-height: 100rpx;
	}
	.infoLabel{
	  margin-top: 20rpx;
	  margin-left: 30rpx;
	  font-size: 28rpx;
	  color: #f65535;
	}
	.shiliLabel{
	  margin-top: 30rpx;
	  margin-left: 30rpx;
	  font-size: 36rpx;
	  color: #000;
	}
	.shiliView{
	  padding:30rpx 30rpx;
	  width: 100%;
	}
	.shiliView image{
	  width: 30%;
	  height: 100%;
	  /* height: width/3; */
	  margin-left: 3%;
	  border-radius: 5%;
	}
	.weui-uploader__bd {
	  margin-bottom: -4px;
	  margin-right: -9px;
	  overflow: hidden;
	}
	.weui-uploader__file {
	  float: left;
	  margin-right: 15px;
	  margin-bottom: 9px;
	}
	.image_view_css{
	  width:60rpx;
	  height: 60rpx;
	  position: absolute;
	  z-index: 5;
	  
	  margin-top: -25rpx
	}
	.weui-uploader__img {
	  /* display: block; */
	  margin-left: 10rpx;
		width: 95px;
	  height: 95px;
	  border-radius: 5%;
	}
	.weui-uploader__input-box {
	  float: left;
	  position: relative;
	  margin-right: 9px;
	  margin-bottom: 9px;
	  width: 95px;
	  height: 95px;
	  border: 1px solid #d9d9d9;
	  margin-left: 10rpx;
	}
	.weui-uploader__input-box:before, .weui-uploader__input-box:after {
	  content: " ";
	  position: absolute;
	  top: 50%;
	  left: 50%;
	  transform: translate(-50%, -50%);
	  background-color: #d9d9d9;
	}
	.weui-uploader__input-box:before {
	  width: 2px;
	  height: 39.5px;
	}
	.weui-uploader__input-box:after {
	  width: 39.5px;
	  height: 2px;
	}
	.weui-uploader__input {
	  position: absolute;
	  z-index: 1;
	  top: 0;
	  left: 0;
	  width: 100%;
	  height: 100%;
	  opacity: 0;
	}
	.hideTrue {
	  display: none
	}
	.btnView{
	  margin-top: 0rpx;
	  padding:30rpx 30rpx;
	  margin-left: 30rpx;
	  margin-right: 30rpx;
	  margin-bottom: 30rpx;
	  height: 100rpx;
	  background-color: #f65535;
	  color: white;
	  border-radius: 50rpx;
	  display: flex;
	  justify-content: center;
	  align-items: center;
	}
</style>
