<template>
	<view>
      <swiper :current = "current"  style="" @change="changeSwiper">
          <block v-for="(item , index) in imgs" :key="index">
             <swiper-item class="swiper-items" >
               <image class="swiper-image" :src="item" mode="scaleToFill"></image>
             </swiper-item>
          </block>
       </swiper>
        <button class="button-img" @click="start" v-if=" current == imgs.length - 1">立即进入</button>
        <button class="button-img" @click="next" v-if=" current < imgs.length - 1">下一步</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				 imgs: [
				      "../../static/guide_1.png",
				      "../../static/guide_2.png",
				      "../../static/guide_3.png",
				   ],
				   current:0,
				   isflog:'',
				
			}
		},
		onLoad(option) {
			this.isflog = option.isflog;
			console.log(this.isflog);
			
		},
		methods:{
			next:function(){
			  let that = this;
         that.current = that.current + 1;
	
			 },
			 changeSwiper:function(e){
				this.current = e.detail.current;
				
			 },
			start() {
			
			  if(this.isflog == '1'){
          uni.switchTab({
            url:'../index/index'
          })
			  }else{
				  uni.switchTab({
				  	url:'../index/index'
				  })
			  }
			}
			
		}
	}
</script>

<style>
	
	swiper {
	  /*这个是绝对布局*/
	   position: absolute;
	   height: calc(100% - 200rpx);
	   width: 100%;
	 } 
	 .swiper-image {
	  height: 100%;
	  width: 100%;
	}
	  
	.button-img {
	    /*这个是绝对布局*/
	  position: fixed;
	  bottom: 0px;
	  height: 100px;
	  margin-left: 50rpx;
	  width: calc(100% - 100rpx);
	  background: #f65535;
	  border-radius: 50rpx;
	 font-size: 36rpx;
	  box-sizing: border-box;
	  height: 100rpx;
	  line-height: 100rpx;
	  color: white;
	}
</style>
