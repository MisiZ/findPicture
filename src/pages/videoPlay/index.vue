<template>
  <view class="video_wrap">
    <image :src="videoList.img"></image>
    <view class="video_tap">
      <view :class="['iconfont',muted?'icon-jingyin':'icon-shengyin']" @click="handleMuted"></view>
      <view class="iconfont icon-zhuanfa">
        <button open-type="share"></button>
      </view>
    </view>
    <view class="video_look">
      <video :src="videoList.video" object-fit="fill" :muted="muted"></video> 
    </view>
    <view class="video_btn">
      <view class="item_btn" @click="handleDownLoad">下载高清
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data(){
    return{
      videoList:[],
      //是否静音,默认否
      muted:false
    }      
  },
  onLoad(){
   this.videoList = getApp().globalData.videoList
   
  },
  methods:{
     handleMuted(){
       this.muted = !this.muted;
     }, 
     //下载视频事件
    async handleDownLoad(){
      await uni.showLoading({
        title:"下载中"
      })
       const {tempFilePath} = (await uni.downloadFile({url:this.videoList.video}))[1]
       await uni.saveVideoToPhotosAlbum({
         filePath: tempFilePath
       });
       await uni.showToast({
         title: '下载完成',
         icon: 'none'
       });
         
         
    }
  }
}
</script>

<style scoped lang="scss">
 .video_wrap {
   position: relative;
  image {
    position: absolute;
    width: 100vw;
    height: 100vh;
    filter: blur(20rpx);
    z-index: -1;
  }
  .video_tap {
    height: 80rpx;
    display: flex;
    justify-content: flex-end;
    .iconfont {
      width: 80rpx;
      height: 80rpx;
      background-color:rgba(0,0,0,.4);
      border-radius: 50%;
      display: flex;
      margin-right: 20rpx;
      justify-content: center;
      align-items: center;
      color: #fff;
    }
    .icon-zhuanfa{
      position: relative;
      button{
        position: absolute;
        width: 100%;
        height: 100%;
        opacity: 0;
      }
    }
  }
  .video_look {
    display: flex;
    justify-content: center;
    video {
    width: 480rpx;
    height: 680rpx;
    }
  }
  .video_btn {
    display: flex;
    justify-content: center;
    .item_btn {
      width: 480rpx;
      height: 80rpx;
      font-size: 34rpx;
      color: #fff;
      margin-top: 20rpx;
      border: 4rpx solid #fff;
      background-color:rgba(0,0,0,.4);
      border-radius: 20rpx;
      display: flex;
      justify-content: center;
      align-items: center;
    }
  }
}
</style>