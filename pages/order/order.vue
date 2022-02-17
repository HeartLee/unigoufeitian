<template>
  <view style="width: 100%; height: 100%">
    <scroll-view
      style="
        margin-top: 80rpx;
        height: calc(100%-100rpx);
        background-color: #f5f5f5;
      "
      class="scroll"
      id="scrollview"
      scroll-y="true"
    >
      <!-- 列表 -->
      <view class="listBox" v-for="(item, index) in tabContent" :key="index">
        <view class="shopname">{{ item.shopname }}</view>
        <view class="listTop">
          <image :src="item.shophead" class="goodsImg"></image>
          <view class="infor">
            <view class="shoptopView">
              <text class="name">{{ item.type }}</text>
              <text class="price" v-if="item.status == 1">待上传</text>
              <text class="price" v-if="item.status == 2">待审核</text>
              <text class="price" v-if="item.status == 3">已完成</text>
              <text class="price" v-if="item.status == 4">已取消</text>
              <text class="price" v-if="item.status == 9">已失效</text>
            </view>
            <view style="margin-top: 10rpx">
              <!-- <text class='choose'>{{item.disconttitle}}</text> -->
              <text class="number">{{ item.disconttitle }}</text>
            </view>
            <view class="dess">
              <text class="notice">{{ item.descc }}</text>
              <!-- <text class='allPrice'>¥{{item.money}}</text> -->
            </view>
          </view>
        </view>
        <view class="line"></view>
        <!-- <view class='listBottom'>
	  <view>共{{item.number}}件商品,合计：¥{{item.allPrice}}</view> -->
        <view
          class="status"
          v-if="item.status == 2"
          style="margin-left: calc(100% - 340rpx)"
        >
          <button
            style="color: #fa3534; border: 1px solid #fa3534"
            @click="gotodetail(item.id)"
          >
            查看详情
          </button>
          <button
            style="color: #fa3534; border: 1px solid #fa3534"
            @click="uploadfile(item.id)"
          >
            上传资料
          </button>
        </view>
        <view
          class="status"
          v-if="item.status == 1"
          style="margin-left: calc(100% - 510rpx)"
        >
          <button
            style="color: #666666; border: 1px solid #666666"
            @click="cancelOrder(item.id)"
          >
            取消订单
          </button>
          <button
            style="color: #fa3534; border: 1px solid #fa3534"
            @click="gotodetail(item.id)"
          >
            查看详情
          </button>
          <button
            style="color: #fa3534; border: 1px solid #fa3534"
            @click="uploadfile(item.id)"
          >
            上传资料
          </button>
        </view>
        <view
          class="status"
          v-if="item.status == 3"
          style="margin-left: calc(100% - 340rpx)"
        >
          <button
            style="color: #fa3534; border: 1px solid #fa3534"
            @click="gotodetail(item.id)"
          >
            查看详情
          </button>
        </view>
        <view
          class="status"
          v-if="item.status == 9"
          style="margin-left: calc(100% - 340rpx)"
        >
          <button
            style="color: #fa3534; border: 1px solid #fa3534"
            @click="gotodetail(item.id)"
          >
            查看详情
          </button>
          <button
            style="color: #fa3534; border: 1px solid #fa3534"
            @click="uploadfile(item.id)"
          >
            上传资料
          </button>
        </view>
        <view
          class="status"
          v-if="item.status == 4"
          style="margin-left: calc(100% - 340rpx)"
        >
          <button
            style="color: #fa3534; border: 1px solid #fa3534"
            @click="gotodetail(item.id)"
          >
            查看详情
          </button>
        </view>
        <!-- </view>  -->
      </view>
    </scroll-view>

    <view class="navbar-box">
      <block v-for="(item, index) in recordMain" :key="index">
        <view
          class="nav-item"
          data-current="item.title"
          @click="switchNav(index)"
        >
          <text :class="{ active: currentTab == index }">{{ item.title }}</text>
        </view>
      </block>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      recordMain: [
        {
          title: "全部",
        },
        {
          title: "待上传",
        },
        {
          title: "待审核",
        },
        {
          title: "已完成",
        },
        {
          title: "已失效",
        },
      ],
      tabContent: [],
      status: "",
      page: 1,
      currentTab: 0,
      navScrollLeft: 0,
      winWidth: 0,
      winHeight: 0,
      isRefreshing: false,
      isFinish: false,
    };
  },
  onShow() {
    this.checkLoginStatus();
  },
  onLoad() {
    this.getmyOrder();
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
            debugger;
            const { confirm, cancel } = res;
            if (confirm) {
              uni.navigateTo({
                url: "../login/login",
              });
            } else if (cancel) {
              uni.switchTab({
                url: "../index/index",
              });
            }
          },
        });
        return false;
      }
      return true;
    },
    switchNav: function (e) {
      var that = this;
      if (e == 0) {
        that.status = "";
      } else if (e == 4) {
        that.status = "9";
      } else {
        that.status = e;
      }
      that.page = 1;
      that.tabContent = [];
      that.currentTab = e;
      that.getmyOrder();
    },
    getmyOrder: function (e) {
      var _this = this;
      wx.request({
        url: "https://srxadmin.goufeitian.com/index.php/Api/Index/getOrderRecord",
        data: {
          openid: localStorage.getItem("opendid"),
          page: _this.page,
          status: _this.status,
        },
        success: function (res) {
          var content = res.data.data;
          var content = res.data.data;
          if (_this.page == 1) {
            _this.tabContent = content;
          } else {
          }
        },
      });
    },

    cancelOrder: function (orderid) {
      var _this = this;
      wx.request({
        url: "https://srxadmin.goufeitian.com/index.php/Api/Index/cancelOrder",
        data: {
          orderid: orderid,
        },
        success: function (res) {
          uni.showToast({
            icon: "none",
            title: "取消成功",
            duration: 2000,
          });
          _this.getmyOrder();
        },
      });
    },
    gotodetail: function (e) {
      uni.navigateTo({
        url: "../../pages/detailpage/detailpage?orderid=" + e,
      });
    },
    uploadfile: function (e) {
      uni.navigateTo({
        url: "../../pages/updatum/updatum?orderid=" + e,
      });
    },
  },
};
</script>

<style>
::-webkit-scrollbar {
  width: 100%;
  height: 100%;
  color: transparent;
}

.navbar-box {
  width: 100%;
  height: 70rpx;
  line-height: 70rpx;
  position: fixed;
  top: 0rpx;
  background: white;
}

.nav-item {
  display: inline-block;
  width: 20%;
  text-align: center;
  color: #666;
}

.nav-item text {
  padding-bottom: 10rpx;
}

page {
  background: #f2f2f2;
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
  padding: 30rpx;
  width: calc(100% - 60rpx);
  margin-left: 30rpx;
  margin-top: 30rpx;
  background: white;
  box-sizing: border-box;
  border-radius: 8rpx;
}

.listTop {
  display: flex;
  margin-top: 20rpx;
  /* justify-content: space-between; */
}

.goodsImg {
  width: 130rpx;
  height: 130rpx;
  margin-right: 20rpx;
  border-radius: 20rpx;
}

.infor {
  padding: 10rpx 0rpx 10rpx 0rpx;
  font-size: 26rpx;
  margin-top: 20rpx;
  color: #666;
  width: calc(100% - 150rpx);
}

.shoptopView {
  align-items: center;
  display: flex;
  flex-direction: row;
}
.name {
  display: inline-block;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  color: #fa3534;
}

.choose {
  display: inline-block;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  margin-top: 10rpx;
}
.price {
  font-size: 28rpx;
  font-weight: bold;
  color: #666;
  margin-right: 60rpx;
  right: 0;
  position: absolute;
}
.number {
  /* padding: 5rpx 10rpx; */
  margin-top: 10rpx;
  box-sizing: border-box;
}
.dess {
  margin-top: 10rpx;
  display: flex;
  flex-direction: row;
}
.notice {
  display: inline-block;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  color: #fa3534;
  margin-right: 100rpx;
}
.allPrice {
  color: #fa3534;
  font-size: 32rpx;
  font-weight: bold;
  margin-right: 60rpx;
  right: 0;
  position: absolute;
}
.line {
  width: 100%;
  margin-top: 20rpx;
  height: 2rpx;
  background-color: #cccccc;
  /* border-top:1px #989898 solid; */
}
/* .listBottom {
  text-align: right;
  margin-left: 10rpx;
  margin-top: 20rpx;
 } */

button::after {
  border: none;
}
.shopname {
  margin-left: 5rpx;
  margin-top: 20rpx;
  color: #4e4d4d;
  font-size: 30rpx;
  font-weight: bold;
}
.status {
  margin-left: calc(100% - 500rpx);
  /* margin-left: 25%; */
  padding: 20rpx 0rpx 0rpx 0rpx;
  position: relative;
  display: flex;
  flex-direction: row;
}
.status button {
  display: inline-block;
  background: white;
  border: 1px solid #dedede;
  border-radius: 66rpx;
  font-size: 24rpx;
  width: 150rpx;
  color: #666;
  box-sizing: border-box;
  height: 50rpx;
  line-height: 45rpx;
  margin-right: 0rpx;
  padding: 0rpx;
}
</style>
