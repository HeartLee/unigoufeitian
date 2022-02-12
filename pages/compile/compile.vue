<template>
	<view >
		<view class="userHeader">
		  <view class="userimage">
		  <image  @click="headimage" :src="head" style="width:100%;height:100%; border-radius: 125rpx;" mode="aspecFill" ></image>
		     <view class="userimgeBtn" @click="headimage">点击修改</view>
		  </view>
		
		</view>
		<view class="phoneView">
		  <view class="phoneViewbox"> 
		    <view class="phoneViewtitle"> 昵称</view>
		     <input v-model="userInfo.nickname" @input="getuserInfo" placeholder="请输入联系人" style="font-size:30rpx; margin-right:30rpx;height:98rpx;width:300rpx;text-align:right;position: absolute; right: 0" ></input>
		   </view>
		    <view class="linview"></view>
		    <view class="phoneViewbox"> 
		    <view class="phoneViewtitle"> 性别</view>
		
		    <radio-group class="radio-group" @change="radioChange" style="font-size:30rpx; margin-right:30rpx;height:98rpx;width:300rpx;text-align:right;position: absolute; right: 0;line-height:98rpx" >
		    <label class="radio" v-for="(item,index) in items"  style=" margin-left:30rpx;">
		        <radio  v-model="item.value" checked="item.checked"/>{{item.value}}
		    </label>
		</radio-group>
		   </view>
		   <view class="linview"></view>
		     <view class="phoneViewbox"> 
		    <view class="phoneViewtitle"> 生日</view>
		      <picker mode="date" :value="arriveToDate"   @change="changeDate2" style="font-size:30rpx; margin-right:30rpx;height:98rpx;width:300rpx;text-align:right;position: absolute; right: 0;line-height:98rpx" >
            <view class="picker">
              {{arriveToDate}}
            </view>
         </picker>
		   </view>
		   <view class="linview"></view>
		  </view>
		  <view class="btnView"  @click="submitClick">提交</view>
		
	</view>	   
</template>

<script>
export default {
	data() {
		return {
			 username:'',
			  head:"",
			  arriveToDate:'',
        date:'',
			  userInfo:{},
			  sex:'',
			  items: [
			    { value: '男',checked: 'true'},
			    { value: '女' }],
		}
	},
	onLoad(option) {
	
		this.getPersoninfo()
	},
	methods:{

		  changeDate2:function(e){
		    console.log(e.detail.value);
			  this.arriveToDate = e.detail.value

		  },
		radioChange: function(e) {
			 this.sex=e.detail.value
		
		  },
		getuserInfo:function(e){
			this.username = e.detail.value;
		},
		
		headimage:function() {
		      var that = this;
          uni.chooseImage({
            success: (res) => {
              let tempFilePaths = res.tempFilePaths;
              tempFilePaths.forEach((item) => {
                uni.uploadFile({
                  url: 'https://srxadmin.goufeitian.com/index.php/Api/Index/imagesupload', //服务器地址
                  fileType: "image", //ZFB必填,不然报错
                  filePath: item, //这个就是我们上面拍照返回或者先中照片返回的数组
                  name: 'file',
                  success: (uploadFileRes) => {
                    that.head = JSON.parse(uploadFileRes.data).data;
                  }
                });
              })
            }
          });
		  },
		  getPersoninfo: function (options) {
		    var that = this;
		    uni.request({
		        url: 'https://srxadmin.goufeitian.com/index.php/Api/Index/getPersoninfo',
		        data: {
		             openid:localStorage.getItem("opendid")
		        },
		        success: function (res) {
		            if (res.data.status == '1') {
		                if (!res.data.data.nickname) {
		                }
                      that.userInfo = res.data.data;
                      that.head = that.userInfo.avaimg;
                      that.username = that.userInfo.nickname;
                      that.arriveToDate = that.userInfo.birth;
		               
		                if(res.data.data.sex=='女'){
                        that.sex = '女';
                        that.items = [{ value: '男'},{ value: '女',checked: 'true'}];
		                }else{
		                 that.sex = '男';
		                 that.items = [{ value: '男',checked: 'true'},
		                     { value: '女'}];
		                }
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
		submitClick:function(){
		    
		    var that = this;
        uni.request({
          url: 'https://srxadmin.goufeitian.com/index.php/Api/Index/updatePerson',

          data: {
            openid: localStorage.getItem("opendid"),
            sex:that.sex,
            nickname:that.username,
            avatarUrl:that.head,
            birth:that.arriveToDate,
          },
          success: function (res) {
            if(res.data.status=='1'){
              uni.showToast({
                title: '更新成功',
                icon:'none'
              })
              uni.navigateBack();
            }
          },
        })
		  },
	}
}
</script>
<style>
	.userHeader{
	  margin-top: 60rpx;
	  height: 250rpx;
	  width: 100%;
	  align-items: center;
	  display: flex;
	  flex-flow: column;
	  align-items: center;
	}
	.userimage{
	  background-color: aqua;
	  border-radius: 125rpx;
	  height: 100%;
	  width: 250rpx;
	}
	.userimgeBtn{
	  top: 0rpx;
	  position: absolute;
	  margin-top: 60rpx;
	  height: 250rpx;
	  width: 250rpx;
	  line-height: 250rpx;
	  color: #333;
	  font-size: 34rpx;  
	  display: flex;
	  justify-content: center;
	}
	.phoneView{
	  position: absolute; 
	  margin-top: 30rpx;
	  left: 30rpx;
	  right: 30rpx;
	  height: 400rpx;
	  background-color: #fff;
	  border-radius:25rpx;
	  box-shadow:0.5px 1px 1px 1px #e1e1e1;
	}
	.phoneViewbox{
	  width: 100%;
	  height: 130rpx;
	  display: flex;
	  flex-direction:column;
	  justify-content: center;
	}
	.phoneViewtitle{
	  margin-left: 40rpx;
	  font-size: 28rpx;
	  color: #333333;
	  height: 100rpx;
	  display: flex;
	  flex-direction:column;
	  justify-content: center;
	}
	.linview{
	  margin-top: 2rpx;
	  margin-inline: 30rpx;
	  height: 2rpx;
	  background-color: #f5f5f5;
	}
	.line {
	  margin-top: 2rpx;
	  margin-left: 30rpx;
	  margin-right: 30rpx;
	  height: 2rpx;
	  background-color: #f5f5f5;
	}
	.buttonView{
	  display: flex;
	  height: 100rpx;
	  justify-content: center;
	  align-items: center;
	  /* width: 200rpx; */
	  position: absolute; 
	  right: 0; /* 靠右调节 */
	  margin-right: 60rpx;
	  color: #f65535;
	}
	.btnView{
	  position: absolute; 
	  bottom: 20rpx;
	  left: 30rpx;
	  right: 30rpx;
	  height: 100rpx;
	  background-color: #f65535;
	  color: white;
	  border-radius: 50rpx;
	  display: flex;
	  justify-content: center;
	  align-items: center;
	}
</style>
