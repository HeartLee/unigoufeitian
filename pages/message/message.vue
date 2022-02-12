<template>

		<view>
		   <block v-for="(item,index) in datalist" >
		    <view class="listBox" >
		      <text class="listBoxText">{{item.title}}</text>
		     <text class="listBoxsubTitle">{{item.descc}}</text>
		    </view>
		  <view class="line"></view>
		  
		<!-- 弹出对话框结束 -->
		</block>

	</view>
</template>

<script>
	export default {
		data() {
			return {
			  datalist: [
			    ],
				
			}
		},
		onLoad(option) {
			  this.getNotice();
			
		},
		methods:{
			 getNotice: function(){
			    var  that= this;
			    uni.request({
			      url: 'https://srxadmin.goufeitian.com/index.php/Api/Index/getNotice',
			      data: {
			      },
			      success: function (res) {
			        var content = res.data.data;
					that.datalist = content;
			    
			      },
			  })
			  },
		},
	}
</script>

<style>
	/* pages/message/message.wxss */
	page {
	  background-color: rgb(245, 245, 245);
	}
	.listBox{
	  margin-top: 0rpx;
	  margin-left: 0rpx;
	  background-color: #fff;
	  display: flex;
	  flex-direction:column;
	}
	.listBoxText{
	  margin-left: 40rpx;
	  margin-top: 20rpx;
	  font-size: 32rpx;
	  color: #333;
	  font-weight:bold;
	}
	.listBoxsubTitle{
	  margin-left: 40rpx;
	  margin-top: 20rpx;
	  font-size: 28rpx;
	  color: #666;
	  margin-bottom: 20rpx;
	}
	.line {
	  width: 100%;
	  height: 2rpx;
	  background: #f5f5f5;
	  margin-left: 0rpx;
	}
	.boomView{
	  width: 100%;
	  height: 30rpx;
	  background: #f5f5f5;
	  margin-left: 0rpx;
	}
</style>
