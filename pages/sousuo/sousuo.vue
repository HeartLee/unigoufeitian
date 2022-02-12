<template>
	<view>
		<view class="deekdianpu">
		    <view class="deek">
		        <view class="searchdp">
		            <image src="../../static/icon-search.png"  style="width: 40rpx;height: 40rpx;"></image>
		        </view>
		        <view class="searchname">
		            <input type="text" placeholder="搜索店铺" @input="editName"></input>
		        </view>
		         <view class="shantu">
		           <image src="../../static/shanchu.png" mode="widthFix"></image>
		        </view>
		    </view>
		</view>
		<view style="background-color:#FFFFFF;width: 100%;">
			<scroll-view scroll-x="true" class="navbar-box">
				<block v-for="(item , index) in recordMain" :key="index">
				  <view class="nav-item" data-current="item.title"  @click="switchNav(index)">
					  <text  :class="{'active':currentTab == index}">{{item.title}}</text>
				  </view>
				</block>
			 </scroll-view>
		</view>
		
		<block v-for="(item , index) in tabContent" :key="index">
		  <!-- 列表 -->
		  <view class='listBox'> 
		   <view class='listTop'>
		      <image mode="widthFix" :src="item.head" @click="godoShopinfo(item.id)" ></image>
		      <view class="dianname" >
		          <view class="namepu"  @click="godoShopinfo(item.id)"  >{{item.title}}</view>
		          <!-- <view class="diantime">9:00-22:00</view> -->
		          <block  v-for="(item2 , index) in item.discont">
		            <view class='listBottom'> 
		              <view class="flex flex-wrap">
		                <view class="flex  radius" style="justify-content: flex-start;align-items: center;">
		              
		                 <view class="desname" style="text-align:left;margin-top:10rpx;" >
							 <image src="../../static/elment.png" style="width:25rpx;height:25rpx;margin-left:10rpx;" v-if="item2.type=='饿了么'"></image>
							 <image src="../../static/meituan.png" style="width:25rpx;height:25rpx;margin-left:10rpx;" v-if="item2.type=='美团'"></image>
							<span style="margin-left:15rpx; margin-top: 20rpx;">距离{{item2.distance}}km</span>
							<span class="diantime">{{item2.iswork}}</span>
		                 </view> 
		                </view>
						   <view  style="margin-top: 30rpx;  flex-direction: row; display: flex;width:100%;padding:0 0 10rpx 0" >
						                  <view  v-if="item2.type=='美团'" style="width:70%; text-align: left;margin-top:10rpx;">
						                        <view style="font-size:22rpx;color:#FBC715" >会员满{{item2.cost}}奖励{{item2.vipfanli}}积分</view>
						                        <view style="font-size:22rpx;color:#FBC715" >非会员满{{item2.cost}}奖励{{item2.fanli}}积分</view>
						                      </view>
						                      <view  v-if="item2.type=='饿了么'" style="width:70%; text-align: left;margin-top:10rpx;">
						                        <view style="font-size:22rpx;color:#3399ff" >会员满{{item2.cost}}奖励{{item2.vipfanli}}积分</view>
						                        <view style="font-size:22rpx;color:#3399ff" >非会员满{{item2.cost}}奖励{{item2.fanli}}积分</view>
						                    </view>
						                <view class="radius"  style="margin-left:10%;width:35%;">
						                  <view class="minge" v-if="item2.type=='美团'">剩{{item2.remain_num}}名
						                 </view>
						                  <view class="minge" v-if="item2.type=='饿了么'" style="color:#3399ff">
						                      剩{{item2.remain_num}}名
						                  </view>
						                  <view class='status' v-if="item2.remain_num=='0'">
						                    <button class='btn1'  style="color: #fff;background:#ccc;">
						                        已售罄
						                    </button>
						                  </view> 
						                  <view class='status' v-if="item2.remain_num!='0'">
						                    <button class='btn1' v-if="item2.type=='美团' "  @click="submitinfo(item.shopid,item2)" style="background:#FBC715">
						                     <view>
						                      抢购
						                      </view>
						                    </button>
						                     <button class='btn1' v-if="item2.type=='饿了么'"  @click="submitinfo(item.shopid,item2)"   style="background:#3399ff">
						                      <view >
						                        抢购
						                      </view>
						                    </button>
						                  </view> 
						                </view> 
						               </view>
		           </view>
				   </view>
		          </block>
		      </view>
		   </view>
		  
		  </view>
		 </block>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				  currentTab: 0,
				    long:0,
				    lat:0,
				    page:1,
				    iship:'',
				    typeid:'0',
				    keywords:'',
				    city:'',
				    recordMain: [
				      {
				        title: "免费探店-全部"
				      },
				      {
				        title: "可配送"
				      }
				     ],
				     tabContent: [],
				
			}
		},
		onLoad(option) {
			console.log( option.opion);
			let  opion = JSON.parse(option.opion)
			var that = this;
			console.log(opion);
			that.lat = opion.lat;
			that.long = opion.long;
			that.city = opion.city;
			
			this.getShopList();	
		},
		methods:{
			switchNav:function(e) {
			var _this = this;
			    if (_this.currentTab == e) {
			    } else {
			      if(e==0) {
					_this.iship = '';
					_this.page = 1;
			      }else{
					_this.iship = '1';
					_this.page = 1;
			      }
					_this.currentTab = e;
			    }
			},
			godoShopinfo:function(e) {
				console.log(e);
				var that = this
				let opion ={
					'storeid': e,
					'long':that.long,
					'lat':that.lat,
					'isvip':that.isvip,
					 }
								   
				 uni.navigateTo({
					url:'../shopinfo/shopinfo?opion=' + JSON.stringify(opion)
				})
			},
			 editName: function (e) {
			    var that = this;
				that.keywords = e.detail.value;
				that.page = '1';
				that.tabContent = [];
			    that.getShopList();
			  },
			getShopList:function(){
					var _this = this;
					wx.request({
					    url: 'https://srxadmin.goufeitian.com/index.php/Api/Index/getShopList',
					    data: {
					    lng:_this.long,
					    lat:_this.lat,
					    iship: _this.iship,
					    keywords:_this.keywords,
					    typeid:_this.typeid,
					    page:_this.page,
					    city:_this.city,
					 },
						success:function (res) {
					    var content = res.data.data;
						
						_this.tabContent = content;
						}
					    })
					  },
			submitinfo:function(id,item){
				this.shopid = id,
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
					  		
					  	 uni.showModal({
					  	    title: '提示',
					  	    content: ' 预售订单,活动时间'+ string+'  '+e.currentTarget.dataset.list.isdiscont_start + '开始',
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
					  		        openid:localStorage.getItem("opendid"),
									discontid:item.id,
					  		    },     
					  		    success:function (res) {
					  		         var content = res.data.data;
					  		        console.log(content);
					  		        if(res.data.status=='1'){
										uni.showToast({
											icon:'none',
											title:'抢购成功，请点击店铺头像进入店铺详情页，点击跳转到外卖平台，扫描店铺小程序码进入点餐',
											duration:2000,
										})
										setTimeout(function () {
												 uni.switchTab({
												   url: '../../pages/order/order'
												 })
										}, 2000);
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
	/* pages/sousuo/sousuo.wxss */
	.deekdianpu{
	  margin-top: 0rpx;
	  background-color: #fff;
	  padding:50rpx 30rpx;
	}
	.deekdianpu .deek{
	  box-shadow:0px 2px 5px 2px #eee;
	  height: 100rpx;
	  padding-left: 40rpx;
	  align-items: center;
	  border-radius: 50rpx;
	  display: flex;
	}
	.search_ipt input {
	  height: 70rpx;
	  background: none;
	  outline: none;
	  width: 100%;
	}
	
	.searchdp {
	  display: flex;
	  align-items: center;
	  height: 70rpx;
	}
	
	.searchname {
	  display: flex;
	  align-items: center;
	  padding-left: 60rpx;
	  padding-right: 60rpx;
	  width: 100%;
	}
	
	.searchdp image {
	  width: 50rpx;
	  position: absolute;
	  padding-left: 10rpx;
	}
	.shantu image {
	  width: 50rpx;
	  margin-right: 30rpx;
	}
	.deekdianpu .deekdianpu{
	  margin-top: 50rpx;
	  width: 100%;
	  background-color:rgb(252, 249, 240) ;
	  border-radius: 35rpx;
	  display: flex;  
	  flex-direction: row; 
	  align-items: center;
	  padding: 2%;
	}
	.deekdianpu .boxtitle{
	  font-size: 24;
	  color: #333;
	  width: 33%;
	  margin-left: 2%;
	  text-align: center;
	}
	/*列表*/
	.listBox {
	  margin-top: 20rpx;
	 border-radius: 14rpx;
	 padding: 30rpx 25rpx; 
	 margin-left: 30rpx;
	 width: calc(100% - 60rpx);
	
	 /* margin-top: 30rpx; */
	 background-color: #FFFFFF;
	 box-sizing: border-box;
	 border-radius: 8rpx;
	 box-shadow:0px 2px 2px 2px #eeeeee;
	} 
	.nav-item text {
	  padding-bottom: 10rpx;
	  font-size: 28rpx;
	 }
	  
	  
	 .active {
	  color: #f65535;
	  border-bottom: 4rpx solid #f65535;
	  font-size: 28rpx;
	  box-sizing: border-box;
	 }
	.navbar-box {
	  line-height: 70rpx;
	  /* position: fixed; */
	  padding: 20rpx 20rpx;
	  /* background: white; */
	  color: #666;
	 }
	 .nav-item {
	  display: inline-block;
	  width: 30%;
	  text-align: center;
	 }
	.listBox .listTop{
	 display: inline-block;
	 width: 100%;
	 display: flex;
	 flex-direction: row;
	}
	.listTop image{
	 width: 25%;
	 border-radius: 20rpx;
	}
	.listTop .dianname{
	 padding-left:4%;
	 display: inline-block;
	 width: 71%;
	}
	.dianname .namepu{
	 font-weight: bold;
	 overflow:hidden;
	 /*不换行*/
	 white-space:nowrap;
	 /*添加...*/
	 text-overflow:ellipsis;
	}
	
	.dianname .desname{
	 margin-top: 10rpx;
	 color:#999;
	 font-size:21rpx
	}
	.dianname .diantime{
	 background-color:#f65535 ;
	 padding: 0rpx 10rpx;
	 color: white;
	 margin-left: 30rpx;
	 border-radius: 20rpx;
	 font-size:21rpx
	}
	.listBottom .minge{
	 color:#f65535;
	 text-align: center;
	 margin-right: 10rpx;
	 font-size: 21rpx;
	}
	 
	.infor view {
	 width: 100%;
	 display: flex;
	 justify-content: space-between;
	}
	 
	.infor view:nth-of-type(2) {
	 font-size: 24rpx;
	}
	 
	.name .choose {
	 font-weight: 600;
	 display: inline-block;
	 overflow: hidden;
	 text-overflow: ellipsis;
	 white-space: nowrap;
	 width: 320rpx;
	}
	
	 
	.listBottom {
	 text-align: right; 
	 margin-top: 20rpx;
	
	}
	.listBottom .status{
	 margin-top: 10rpx;
	
	}
	
	.status .btn1 {
	 font-size: 24rpx;  
	 margin-top: 4rpx;
	 height: 50rpx; 
	 color: #333;
	 background:#3399ff;
	 border-radius: 50rpx;
	 display: flex;
	 flex-direction: row;
	 align-items: center;
	 justify-content: center;
	 color: white;
	}
	.flex-wrap { 
	 align-items: flex-end; 
	}
	.padding-sm {
	 padding: 0 20rpx;
	
	}
	/* .listBottom.btnImg {
	 margin-right: 8rpx;
	 width: 46rpx;
	 height: 46rpx;
	} */
	
	.listBottom.btn1::after {
	 border-radius: 98rpx;
	 border: 0; 
	}
</style>
