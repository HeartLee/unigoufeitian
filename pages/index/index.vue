<template style="background-color: #F5F5F5;">
	<view style="width: 100%;height: 100%;">
	<view class="headerView">
	   <view class="frame">
	   				<image  @click="gotosous" src="../../static/icon-search.png" style="width:30rpx;height:30rpx"></image>
	   </view>
	    <view class="slot-wrap">
	       <image src="../../static/dingwei_1.png" style="width:50rpx"mode="widthFix"></image>
	   			<view class="address" @click="choseAddress">{{loctaddress}}</view>
	   </view>
	</view>  
	
	<swiper class="swiper"
	   			indicator-dots="true" 
	   			autoplay="true" 
	   			interval="5000" 
	   			duration="1500"	>
	   			<swiper-item v-for="(item , index) in imgUrls" :key="index">
	   				<image :src="item.imgurl" mode="aspectFill"></image>
	   			</swiper-item>
	</swiper>
	<view class="menuView">
	   <block v-for="(item , index) in typeList">
	      <div class="menuViewlist"  @click="typeClick(item.name)" >
	         <image :src="item.imgurl" style="width:60%" mode="widthFix"></image>
	           <view class="menuViewtitle"  style="color:#666666">{{item.name}}</view>
	     </div>
	  </block>
	</view>
	
	<div style="margin-bottom: 15px; display: none;">
		<div style="display: inline-block;width: 50%;">
			<a href="https://dpurl.cn/VCx8oG7z">
				<img src="../../static/meituan.jpg" style="width:100%"></image>
			</a>
		</div>
		
		<div style="display: inline-block;width: 50%;" @click="elma">
			<a href="#">
				<img src="../../static/elma.jpg" style="width:100%"></image>
			</a>
		</div>
	</div>
	
	  <view class="hostbuss" style="display: none;">
	     <view class="hostbusstitle">优选好店</view>
	      <view class="hostbusslist">
	      <block v-for="(item , index) in ShopListHome">
	            <view class="hostbusslistbox"  @click="goodClick(item.id)" >
	              <image :src="item.head" style="width:90%; border-radius: 10rpx; margin-top: 20rpx" mode="aspectFill" ></image>
	              <view class="hostbusslistTitle">{{item.title}}</view>
	              <view class="hostbusslistTitle">{{item.distance}}km</view>
	          </view>
	     </block>
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
	  <view class='listBox' @click="godoShopinfo(item.id)"> 
	   <view class='listTop'>
	      <image mode="widthFix" :src="item.head"></image>
	      <view class="dianname" >
	          <view class="namepu" >{{item.title}}</view>
	          <!-- <view class="diantime">9:00-22:00</view> -->
	          <block  v-for="(item2 , index) in item.discont">
	            <view class='listBottom'> 
	              <view class="flex flex-wrap">
	                <view class="flex  radius" style="justify-content: flex-start;align-items: center;">
	              
	                 <view class="desname" style="text-align:left;margin-top:10rpx;display: block;" >
						 <image src="../../static/elment.png" style="width:25rpx;height:25rpx;margin-left:10rpx;" v-if="item2.type=='饿了么'"></image>
						 <image src="../../static/meituan.png" style="width:25rpx;height:25rpx;margin-left:10rpx;" v-if="item2.type=='美团'"></image>
						<span style="margin-left:15rpx; margin-top: 20rpx;">距离{{item2.distance}}km</span>
						<span class="diantime">{{item2.iswork}}</span>
	                 </view> 
					<!-- <view v-if="item2.tag" style="display: block;margin-top:10rpx;">
						 <span class="diantime">{{item2.tag}}</span>
					 </view> -->
	                </view>
					   <view  style="margin-top: 30rpx;  flex-direction: row; display: flex;width:100%;padding:0 0 10rpx 0">
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
					                    <button class='btn1' v-if="item2.type=='美团'" style="background:#FBC715">
					                     <view>
					                      去报名
					                      </view>
					                    </button>
					                     <button class='btn1' v-if="item2.type=='饿了么'"  style="background:#3399ff">
					                      <view >
					                        去报名
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

    <!-- <movable-area class="movable-area">
      <movable-view class="movable-view "   direction="vertical" scale="true" y='0'>
          <view class="dianming" @click="showCode" >
              <view class="text" >联系</view>
              <view class="text" style="margin-top: -5rpx;">客服</view>
          </view>
        </movable-view>
    </movable-area> -->

    <view class="mimaBg"  v-if="isshowCode" catchtouchmove='true'>
        <view class="mimaBgcontentView">
        <view class="image1">
          <image src="../../static/qrcode.jpeg" style="width: 100%;" mode="aspectFit"></image>
        </view>
        <view class="title1">
            扫码关注公众号联系客服
        </view>
        <view class="buttonView" @click="showCode1">
            我知道了
        </view>
      </view>
    </view>



    <view class="mimaBg"  v-if="isshowmjd" catchtouchmove='true'>
      <view class="mimaBgcontentView">
        <view class="image1">
          <image src="../../static/qrcode_mjd.png" style="width: 100%;" mode="aspectFit"></image>
        </view>
        <view class="title1">
          <view>等待5秒钟，长按二维码图片可调用微信小程序</view>
		   <view>若未显示二维码，请复制店名在外卖平台搜索下单即可</view>
          <view style="margin-top: 20px;"> <a href="http://seller.yingdiankeji.cn/">点击进入卖家端</a></view>
        </view>
        <view class="buttonView" @click="toggleMjd">
          我知道了
        </view>
      </view>
    </view>


</view>

</template>
<script type="text/javascript" src="https://webapi.amap.com/maps?v=1.4.15&key=6d4fab9a3f5495f8c9792bf760a710ac"></script> 
<script>
export default {
  data() {
    return {
      seleIndex: "0",
      qrcode: 0,
      long: 0,
      lat: 0,
      page: 1,
      iship: "",
      typeid: "0",
      userInfo: {},
      loctaddress: "定位中",
      indicatorDots: true, //是否显示面板指示点
      autoplay: true, //是否自动切换
      interval: 3000, //自动切换时间间隔
      duration: 800, //滑动动画时长
      circular: true, //是否采用衔接滑动
      imgUrls: [],
      ShopListHome: [],
      isvip: "",
      address_city: "",
      typeList: [],
      isshowCode: false,
      isshowmjd: false,
      recordMain: [
        {
          title: "免费探店-全部",
        },
        {
          title: "可配送",
        },
      ],
      tabContent: [],
      currentTab: 0,
      usernickname: null,
      shopid: "",
      goodinfo: {},
    };
  },
  onLoad() {
    // 首页不去验证登录状态
    // let token = localStorage.getItem("token");
    // if(token == null){
    // 	uni.navigateTo({
    // 		url:'../login/login'
    // 	})
    // }

    this.getLocation();
    this.getBanner();
    this.getTypeList();
  },
  methods: {
    checkLoginStatus() {
      const openId = localStorage.getItem("opendid");
      if (!openId) {
        uni.showModal({
          title: "提示",
          content: "您还没登录，请先登录",
          confirmText: "去登录",
          confirmColor: "#f65535",
          success(res) {
            const { confirm } = res;
            if (confirm) {
              uni.navigateTo({
                url: "../login/login",
              });
            }
          },
        });
        return false;
      }
      return true;
    },
    getLocation() {
      let _this = this;
      uni.getLocation({
        type: "wgs84",
        geocode: "yes",
        success: function (res) {
          _this.lat = res.latitude;
          _this.long = res.longitude;

          _this
            .$jsonp(
              "https://apis.map.qq.com/ws/geocoder/v1/?location=" +
                res.latitude +
                "," +
                res.longitude +
                "&key=ASRBZ-KNX3F-TO6J7-NFE4P-QSTRQ-P2B7Q&get_poi=1&output=jsonp"
            )
            .then((res) => {
              _this.loctaddress = res.result.address;
              _this.address_city = res.result.ad_info.city;
              console.log(res, "百度");

              _this.getShopList();
            });
        },
      });
    },
    /*获取banner图* */
    getBanner: function () {
      var _this = this;
      uni.request({
        url: "https://srxadmin.goufeitian.com/index.php/Api/Index/getBanner",
        data: {},
        success: function (res) {
          var content = res.data.data;
          _this.imgUrls = content;
        },
      });
    },
    /**获取类型 */
    getTypeList: function () {
      var _this = this;
      wx.request({
        url: "https://srxadmin.goufeitian.com/index.php/Api/Index/getType",
        data: {},
        success: function (res) {
          var content = res.data.data;
          _this.typeList = content;
        },
      });
    },
    getShopListHome: function () {
      var _this = this;
      wx.request({
        url: "https://srxadmin.goufeitian.com/index.php/Api/Index/getShopListHome",
        data: {
          lng: _this.long,
          lat: _this.lat,
        },
        success: function (res) {
          var content = res.data.data;
          _this.ShopListHome = content;
        },
      });
    },
    goodClick: function (e) {
      var that = this;
      let opion = {
        storeid: e,
        long: that.long,
        lat: that.lat,
        isvip: that.isvip,
      };

      uni.navigateTo({
        url: "../shopinfo/shopinfo?opion=" + JSON.stringify(opion),
      });
    },
    elma: function (e) {
      uni.showToast({
        icon: "none",
        title: "开发中",
        duration: 2000,
      });
    },
    godoShopinfo: function (e) {
      console.log(e);
      var that = this;
      let opion = {
        storeid: e,
        long: that.long,
        lat: that.lat,
        isvip: that.isvip,
      };

      uni.navigateTo({
        url: "../shopinfo/shopinfo?opion=" + JSON.stringify(opion),
      });
    },
    switchNav: function (e) {
      var _this = this;
      if (_this.currentTab == e) {
      } else {
        if (e == 0) {
          _this.iship = "";
          _this.page = 1;
        } else {
          _this.iship = "1";
          _this.page = 1;
        }
        _this.currentTab = e;
      }
      this.getShopList();
    },
    getShopList: function () {
      var _this = this;
      wx.request({
        url: "https://srxadmin.goufeitian.com/index.php/Api/Index/getShopList",
        data: {
          lng: _this.long,
          lat: _this.lat,
          iship: _this.iship,
          keywords: "",
          typeid: _this.typeid,
          page: _this.page,
          city: _this.address_city,
        },

        success: function (res) {
          var content = res.data.data;
          _this.tabContent = content;
        },
      });
    },
    typeClick: function (e) {
      var title = e;
      if (title == "新手指南") {
        uni.navigateTo({
          url: "../wecome/wecome?isflog=1",
        });
      }
      if (title == "商务合作") {
        uni.navigateTo({
          url: "../cooperation/cooperation",
        });
      }
      if (title == "推广有礼") {
        const loginStatus = this.checkLoginStatus();
        if (loginStatus) {
          uni.navigateTo({
            url: "../fenyong/fenyong",
          });
        }
      }
      if (title == "联系客服") {
        this.showCode();
      }
      if (title == "重新定位") {
        this.getLocation();
      }
      if (title == "加入会员") {
        uni.navigateTo({
          url: "../vipbuy/vipbuy",
        });
      }

      if (title == "我的积分") {
        uni.navigateTo({
          url: "../integral/integral",
        });
      }

      if (title == "我的推广") {
        uni.navigateTo({
          url: "../fenyong/fenyong",
        });
      }
      if (title == "商家中心") {
        this.toggleMjd();
      }
    },
    gotosous() {
      var that = this;
      let opion = {
        long: that.long,
        lat: that.lat,
        city: that.address_city,
      };
      uni.navigateTo({
        url: "../sousuo/sousuo?opion=" + JSON.stringify(opion),
      });
    },
    submitinfo: function (id, item) {
      (this.shopid = id), (this.goodinfo = item);
      const loginStatus = this.checkLoginStatus();
      if (loginStatus) {
        if (item.daoqi == "0") {
          var string = "";
          if (item.start_date) {
            string = item.start_date.substring(5, 10);
            string = string.replace("-", ".");
          }

          uni.showModal({
            title: "提示",
            content:
              " 预售订单,活动时间" +
              string +
              "  " +
              item.isdiscont_start +
              "开始",
            confirmText: "确认",
            confirmColor: "#f65535",
            success(res) {},
          });
          return;
        }
        if (!item.isdiscont) {
          uni.showModal({
            title: "提示",
            content: " 预售订单,活动时间" + item.iswork + "开始",
            confirmText: "确认",
            confirmColor: "#f65535",
            success(res) {},
          });
          return;
        }

        uni.showModal({
          title: "提示",
          content:
            "已明确本店铺活动内容及说明，参与抢单后次日早上9 点以前（凌晨夜宵订单，当日早9 点前）必须完成【订单信息】截图并上传至惠食天下霸王餐小程序，否则视为品鉴任务失败，您确认抢单？",
          confirmText: "确认",
          confirmColor: "#f65535",
          success(res) {
            if (res.confirm) {
              uni.request({
                url: "https://srxadmin.goufeitian.com/index.php/Api/Index/submitinfo",
                data: {
                  openid: localStorage.getItem("opendid"),
                  discontid: item.id,
                },

                success: function (res) {
                  var content = res.data.data;
                  console.log(content);
                  if (res.data.status == "1") {
                    uni.showToast({
                      icon: "none",
                      title:
                        "抢购成功，请点击店铺头像进入店铺详情页，点击跳转到外卖平台，扫描店铺小程序码进入点餐",
                      duration: 2000,
                    });
                    setTimeout(function () {
                      uni.switchTab({
                        url: "../../pages/order/order",
                      });
                    }, 2000);
                    // _this.rocessPay(JSON.parse(content))
                  } else {
                    uni.showToast({
                      icon: "none",
                      title: res.data.info,
                    });
                  }
                },
              });
            }
          },
        });
      }
    },
    showCode1() {
      this.isshowCode = false;
    },
    showCode() {
      this.isshowCode = true;
    },
    toggleMjd() {
      if (this.isshowmjd == false) {
        this.isshowmjd = true;
      } else {
        this.isshowmjd = false;
      }
    },
    choseAddress() {
      var that = this;
      uni.chooseLocation({
        success: function (res) {
          that.long = res.longitude;
          that.lat = res.latitude;

          var regex =
            /^(北京市|天津市|重庆市|上海市|香港特别行政区|澳门特别行政区)/;
          var REGION_PROVINCE = [];
          var addressBean = {
            REGION_PROVINCE: null,
            REGION_COUNTRY: null,
            REGION_CITY: null,
            ADDRESS: null,
          };
          function regexAddressBean(address, addressBean) {
            regex = /^(.*?[市州]|.*?地区|.*?特别行政区)(.*?[市区县])(.*?)$/g;
            var addxress = regex.exec(address);
            addressBean.REGION_CITY = addxress[1];
            addressBean.REGION_COUNTRY = addxress[2];
            addressBean.ADDRESS = addxress[3] + "(" + res.name + ")";
            console.log(addxress);
          }
          if (!(REGION_PROVINCE = regex.exec(res.address))) {
            regex = /^(.*?(省|自治区))(.*?)$/;
            REGION_PROVINCE = regex.exec(res.address);
            addressBean.REGION_PROVINCE = REGION_PROVINCE[1];
            regexAddressBean(REGION_PROVINCE[3], addressBean);
          } else {
            addressBean.REGION_PROVINCE = REGION_PROVINCE[1];
            regexAddressBean(res.address, addressBean);
          }
          // if(addressBean.REGION_CITY != that.address_city){
          //   uni.showModal({
          //     title: '提示',
          //     content: '只能定位当前城市，不可切换其他城市',
          //     success: function (res) {
          //       if (res.confirm) {
          //         that.choseAddress();
          //       } else {//这里是点击了取消以后
          //         console.log('用户点击取消')
          //       }
          //     }
          //   })
          // }else{
          that.long = res.longitude;
          that.lat = res.latitude;
          that.loctaddress = addressBean.ADDRESS;
          that.address_city = addressBean.REGION_CITY;
          that.getShopListHome();
          that.getShopList();
          that.loctaddress = addressBean.ADDRESS;
          // }
        },
      });
    },
  },
};
</script>

<style>
.headerView {
  background-color: #f65535;
  display: flex;
  flex-direction: row;
  padding: 20rpx 20rpx;
}
.frame {
  display: flex;
  align-items: center;
  justify-content: center;
  margin-right: 30rpx;
  border-radius: 35rpx;
  margin-left: 5rpx;
  /* margin-top: 20rpx; */
  height: 50rpx;
  padding: 5rpx 5rpx;
}
.slot-wrap {
  width: 800rpx;
  margin-right: 10rpx;
  color: #ffffff;
  align-items: center;
  display: flex;
  text-overflow: ellipsis;
  overflow: hidden;
}
.slot-wrap .address {
  font-size: 24rpx;
  line-height: 50rpx;
  overflow: hidden;
  /*不换行*/
  white-space: nowrap;
  /*添加...*/
  text-overflow: ellipsis;
}
.swiper {
  height: 300upx;
}
swiper-item image {
  width: 100%;
  height: 300upx;
}
.menuView {
  display: flex;
  align-items: center;
  justify-content: center;
  margin-right: 40rpx;
  padding: 40rpx 0rpx;
}
.menuViewlist {
  margin-left: 5%;
  width: 20%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
.menuViewImage {
  width: 80%;
}
.menuViewtitle {
  margin-top: 10rpx;
  font-size: 24rpx;
  color: #666666;
}
.hostbuss {
  margin-left: 20rpx;
  margin-right: 20rpx;
  background-color: #f65535;
  border-radius: 20rpx;
  padding: 30rpx 20rpx 10rpx 20rpx;
}
.hostbusstitle {
  text-align: center;
  font-size: 30rpx;
  color: #fff;
}
.hostbuss .hostbusslist {
  margin-top: 30rpx;
  background-color: #fff;
  border-radius: 20rpx;
  padding: 2%;
  display: flex;
  align-items: center;
  justify-content: center;
}
.hostbusslist .hostbusslistbox {
  display: flex;
  flex-direction: column;
  /* align-items: center; */
  width: 33%;
  padding: 10rpx 10rpx 10rpx 10rpx;
}
.hostbusslistbox image {
  height: 170rpx;
  border-radius: 20rpx;
}
.hostbusslistTitle {
  margin-top: 20rpx;
  text-align: center;
  color: #333;
  font-size: 28rpx;
  text-align: left;
  margin-left: 10rpx;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
.hostbusslistSubTitle {
  margin-top: 10rpx;
  color: #999;
  font-size: 28rpx;
  text-align: left;
  margin-left: 10rpx;
}
::-webkit-scrollbar {
  width: 0;
  height: 0;
  color: transparent;
}

.navbar-box {
  line-height: 70rpx;
  /* position: fixed; */
  padding: 20rpx 20rpx;
  /* background: white; */
  color: #666;
  width: 90%;
}

.nav-item {
  display: inline-block;
  width: 30%;
  text-align: center;
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
.menu {
  font-size: 28rpx;
  width: 100%;
  /* overflow-x: scroll; */
  border-bottom: 20rpx solid #f2f2f2;
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

.listBox {
  margin-top: 20rpx;
  border-radius: 14rpx;
  padding: 30rpx 25rpx;
  margin-left: 30rpx;
  width: calc(100% - 60rpx);

  /* margin-top: 30rpx; */
  background-color: #ffffff;
  box-sizing: border-box;
  border-radius: 8rpx;
  box-shadow: 0px 2px 2px 2px #eeeeee;
}

.listBox .listTop {
  display: inline-block;
  width: 100%;
  display: flex;
  flex-direction: row;
}
.listTop image {
  width: 25%;
  border-radius: 20rpx;
}
.listTop .dianname {
  padding-left: 4%;
  display: inline-block;
  width: 71%;
}
.dianname .namepu {
  font-weight: bold;
  overflow: hidden;
  /*不换行*/
  white-space: nowrap;
  /*添加...*/
  text-overflow: ellipsis;
}

.dianname .desname {
  margin-top: 10rpx;
  color: #999;
  font-size: 21rpx;
}
.dianname .diantime {
  background-color: #f65535;
  padding: 0rpx 10rpx;
  color: white;
  margin-left: 30rpx;
  border-radius: 20rpx;
  font-size: 21rpx;
}
.listBottom .minge {
  color: #fbc715;
  text-align: center;
  margin-right: 10rpx;
  font-size: 21rpx;
  width: 130rpx;
  margin-left: 30rpx;
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
  margin-top: 10rpx;
}
.status {
  margin-top: 0rpx;
}

.status .btn1 {
  font-size: 24rpx;
  margin-top: 4rpx;
  height: 50rpx;
  color: #333;
  background: #3399ff;
  border-radius: 50rpx;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  color: white;
  width: 130rpx;
}
.flex-wrap {
  align-items: flex-start;
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

.dianming {
  margin-top: 30rpx;
  /* box-shadow:0px 0px 4px 4px #cccccc; */
  border-bottom-left-radius: 50rpx;
  border-top-left-radius: 50rpx;
  margin-right: 0rpx;
  padding: 5rpx 10rpx 5rpx 15rpx;
  background-color: #ffffff;
  opacity: 0.9;
}
.dianming .text {
  font-size: 24rpx;
  color: #ff3000;
  text-align: center;
}

.movable-area {
  top: 100rpx;
  right: 0rpx;
  position: fixed;
  margin-right: 0rpx;
  height: 100%;
  width: 120rpx;
  pointer-events: none;
}
.movable-view {
  margin-left: calc(100% - 120rpx);
  position: absolute;
  margin-right: 0rpx;
  width: 120rpx;
  height: 120rpx;
  pointer-events: auto;
}
.aixin-blue {
  margin-top: 20rpx;
}
.movable-img {
  width: 100rpx;
  height: 100rpx;
}
button::after {
  border: 0;
}
.mimaBg {
  top: 0;
  display: flex;
  position: fixed;
  background-color: rgb(0, 0, 0, 0.7);
  height: 100%;
  width: 100%;
  z-index: 999;
}
.contentView {
  margin-top: 200rpx;
  margin-left: 50rpx;
  width: calc(100% - 100rpx);
  height: calc(100% - 400rpx);
  border-radius: 50rpx;
  background-color: #fff;
}
.contentViewTitle {
  width: 100%;
  margin-right: 0rpx;
  margin-top: 30rpx;
  text-align: center;
  color: #000;
  font-size: 40rpx;
}
.status button {
  display: inline-block;
  background: #f65535;
  border-radius: 50rpx;
  font-size: 36rpx;
  color: white;
  margin-left: 30rpx;
  box-sizing: border-box;
  height: 100rpx;
  line-height: 100rpx;
  margin-top: 30rpx;
}
.mimaBg {
  top: 0;
  display: flex;
  position: fixed;
  background-color: rgb(0, 0, 0, 0.7);
  height: 100%;
  width: 100%;
  z-index: 999;
  align-items: center;
}
.mimaBgcontentView {
  margin-left: 50rpx;
  width: calc(100% - 100rpx);
  border-radius: 30rpx;
  background-color: #fff;
}
.mimaBgcontentView .image1 {
  margin-top: 30rpx;
  margin-left: 30rpx;
  margin-right: 30rpx;
}
.mimaBgcontentView .title1 {
  font-size: 24rpx;
  margin-top: 30rpx;
  width: 100%;
  text-align: center;
}
.mimaBgcontentView .buttonView {
  font-size: 24rpx;
  margin-top: 30rpx;
  margin-left: 80rpx;
  margin-right: 80rpx;
  border: 2rpx solid #666666;
  border-radius: 25rpx;
  background-color: #f0f0f0;
  height: 50rpx;
  color: #666666;
  margin-bottom: 30rpx;
  line-height: 50rpx;
  text-align: center;
}
</style>
