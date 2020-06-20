<template>
   <view class="boss_wrap">
     <!-- 用户信息strat -->
       <view class="user_info">
         <view class="user_headImg">
           <image :src="imgDetail.user.avatar"></image>
         </view>
         <view class="user_content">
          <view class="user_name">{{imgDetail.user.name}}</view>
          <view class="user_time">{{imgDetail.btime}}</view>
         </view>
       </view>
     <!-- 用户信息end -->
     <!-- 大图strat -->
     <swiper-action @touchDirection="handleTouchDirection">
       <view class="img_info">
           <image :src="imgDetail.thumb" mode="widthFix"></image>
       </view>
     </swiper-action>
     <!-- 大图end -->
     <!-- 点赞收藏strat -->
       <view class="star_wrap">
         <view class="iconfont icon-dianzan">{{imgDetail.rank}}</view>
         <view class="iconfont icon-shoucang">收藏</view>
       </view>
     <!-- 点赞收藏end -->
     <!-- 专辑开始 -->
     <view class="album_wrap" v-if="album.length">
     <view class="album_item"
           v-for="item in album"
           :key="item.id"
     >
         <view class="album_img">
           <image :src="item.cover" mode="aspectFill"></image>
         </view>
         <view class="album_content">
            <view class="album_title_wrap">
              <view class="album_title">专辑</view>
            </view>
           <text class="album_name">{{item.name}}</text>
         </view>
         <view class="iconfont icon-V"></view>
         </view>
      </view>
     <!-- 专辑结束 -->
     <!-- 最新评论开始 -->
     <view class="comment_wrap" v-if="comment.length" >
       <view class="comment_title">
         <text class="iconfont icon-pinglun"></text>
         <text class="comment_new">最新评论</text>
       </view>
       <view
       class="comment_item"
       v-for="item2 in comment"
       :key="item2.id"
       >
          <view class="comment_img">
            <image :src="item2.user.avatar"></image>
          </view>
          <view class="comment_content">
             <view class="item2_name">{{item2.user.name}}</view>
             <view class="item2_time">{{item2.btime}}</view>
             <view class="item2_content">{{item2.content}}</view>
          </view>
          <view class="iconfont icon-dianzan">{{item2.size}}</view>
       </view>
     </view>
     <!-- 最新评论结束 -->
     
     <!-- 最热评论开始 -->
     <view class="comment_wrap" v-if="hot.length">
       <view class="comment_title">
         <text class="iconfont icon-hot1"></text>
         <text class="comment_new">最热评论</text>
       </view>
       <view
       class="comment_item"
       v-for="item3 in hot"
       :key="item3.id"
       >
          <view class="comment_img">
            <image :src="item3.user.avatar"></image>
          </view>
          <view class="comment_content">
             <view class="item2_name">{{item3.user.name}}</view>
             <view class="item2_time">{{item3.btime}}</view>
             <view class="item2_content">{{item3.content}}</view>
          </view>
          <view class="iconfont icon-dianzan">{{item3.size}}</view>
       </view>
     </view>
     <!-- 最热评论结束 -->
     <!-- 下载图片开始 -->
     <view class="load_img" @click="handleLoadImg">
       <view class="load_btn">下载图片</view>
     </view>
     <!-- 下载图片结束 -->

   </view>
</template>

<script>
import swiperAction from "@/components/swiperAction/swiperAction"
import moment from "moment"
moment.locale("zh-cn");
export default {
  components:{
    swiperAction
  },
  data(){
    return{
       imgDetail:{},
       album: [],
       comment: [],
       hot: [],
       nowIndex:0
    }
  },
  mounted(){
    const {imgIndex} = getApp().globalData
    this.nowIndex = imgIndex;
    this.getList();
  },
  methods:{
    //获取评论数据
    getComment(id){
      this.request({
        url:`http://157.122.54.189:9088/image/v2/wallpaper/wallpaper/${id}/comment`
      }).then(res=>{
        this.album = res.data.res.album
        this.comment = res.data.res.comment 
        this.hot = res.data.res.hot 
        res.data.res.comment.forEach(v=>v.btime=moment(v.atime*1000).fromNow()); 
        res.data.res.hot.forEach(v=>v.btime=moment(v.atime*1000).fromNow()); 
       // this.hot.btime =  moment(res.data.res.hot.forEach(v=>v.atime*1000)).fromNow(); 
        console.log(this.hot.length);
      })
    },
    //重新加载数据
    getList(){
    const {imgList} = getApp().globalData
      this.imgDetail = imgList[this.nowIndex]
      this.imgDetail.btime = moment(this.imgDetail.atime*1000).fromNow(); 
      this.getComment(this.imgDetail.id);
    },
    //图片滑动事件
    handleTouchDirection(e){
      const {imgList} = getApp().globalData 
      if(e.direction==="left"&&this.nowIndex>0){
        this.nowIndex--;
        this.getList()
      }else if(e.direction==="right"&&this.nowIndex-(imgList.length-1)<0){
        this.nowIndex++;
        this.getList()
      }else{
        uni.showToast({
          title:"没有图片了哦",
          icon:"none"
        })
      }
    },
    //下载图片事件
   async handleLoadImg(){
     uni.showLoading({
       title:"下载中"
     })
      const result = await uni.downloadFile({url: this.imgDetail.thumb})
      const result1 = await uni.saveImageToPhotosAlbum({filePath: result[1].tempFilePath}) 
      uni.hideLoading()
      uni.showToast({
       title:"下载成功",
       icon:"none"
     })
    }
  }
}
</script>

<style lang="scss" scoped>
.boss_wrap {
  .user_info {
    display: flex;
    padding: 20rpx;
   .user_headImg {
      
      image{
        width: 100rpx;
        height: 100rpx;
        border-radius: 50%;
      }
    }
    .user_content {
      margin-left: 20rpx;
      .user_name {
        font-weight: 400;
        font-size: 34rpx;
        color:#000;
      }
    }
  }
  .img_info {
    img {
      width: 100%;
    }
  }
  .star_wrap {
    padding: 20rpx;
    display: flex;
    border-bottom: 2rpx solid #ccc;
    .icon-dianzan {
      flex:1;
      display: flex;
      justify-content: center;
      align-items: center;
    }
    .icon-shoucang {
      display: flex;
      align-items: center;
      justify-content: center;
      flex:1;
    }
  }

  .album_wrap {
   .album_item {
     padding: 20rpx;
     border-bottom: 6rpx solid #ccc;
     display: flex;
      .album_img {
        flex:1;
        image {
          width: 180rpx;
          height: 180rpx;
        }
      }
      .album_content{
        flex:3;
        .album_title_wrap{
          border-radius: 10rpx;
          margin-left: 10rpx;
          display: flex;
          justify-content: center;
          align-items: center;
          width: 100rpx;
          height: 50rpx;
          background-color: $color;
          .album_title{
            color: #fff;
          }
        }
        .album_name{
          margin-left: 20rpx;
          font-weight: 400;
          font-size: 34rpx;
          color:#000;
        }
      }
      .icon-V{
        flex:1;
        display: flex;
        justify-content: center;
        align-items: center;
        }
    }
  }

.comment_wrap {
  padding: 10rpx;
  font-size: 32rpx;
  .comment_title {
    padding: 20rpx;
    .icon-hot1 {
      color:$color;
    }
    .comment_new {
      font-size: 36rpx;
      font-weight: 400;
    }
  }
  .comment_item {
    border-bottom: 8rpx solid #ccc;
    display: flex;
    padding: 10rpx;
    .comment_img {
      flex:1;
      image {
        width: 80rpx;
        height: 80rpx;
      }
    }
    .comment_content {
      margin-left: 10rpx;
      flex:10;
      .item2_name {
        color:#ccc;
      }

      .item2_time {
        margin-top: 10rpx;
      }

      .item2_content {
        padding:20rpx 0;
        color: #000;
        font-weight: 500;
        font-size: 34rpx;
      }
    }
    .icon-dianzan {
     flex:1;
     display: flex;
     align-items:flex-end;
    }
  }
}
.icon-pinglun{
  color:rgb(0, 225, 255);
}
.load_img{
  display: flex;
  justify-content: center;
  align-items: center;
  .load_btn{
    text-align: center;
    width: 95%;
    font-size: 38rpx;
    padding: 20rpx;
    color: #fff;
    border-radius: 10rpx;
    background-color: $color;
  }
}

}
</style>