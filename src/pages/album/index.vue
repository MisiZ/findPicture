<template>
  <view v-if="album.length!=0">
    <!-- 图文开始 -->
    <view class="album_wrap">
      <view class="album_item">
         <view class="albun_img">
           <image mode="widthFix" :src="album.lcover"></image>
         </view>
         <view class="album_title">
           <view class="album_name">{{album.name}}</view>
           <view class="album_btn">关注专辑</view>
         </view>
      </view>
      <view class="album_user">
        <view class="user_head_img">
          <view class="user_image"><image :src="album.user.avatar"></image></view>
          <view class="user_name">{{album.user.name}}</view>
        </view>
        <view class="user_desc">{{album.desc}}</view>
      </view>
    </view>
    <!-- 图文结束 -->
    <!-- 列表开始 -->
    <view class="wallpaper_wrap">
      <view class="wallpaper_item"
      v-for="(item,index) in wallpaper"
      :key="item.id"
      >
       <detail-view :list="wallpaper" :index="index">
       <image mode="aspectFill" :src="item.thumb+item.rule.replace('$<Height>',360)"></image>
       </detail-view>
      </view>
    </view>
    <!-- 列表结束 -->
  </view>
</template>

<script>
import detailView from "@/components/Detail";
export default {
  components:{
    detailView
  },
  //获取页面url传递过来的id
  onLoad(options){
   this.id = options.id;
   this.getlist();
  },
  data(){
    return{
      id:-1,
      params:{
        limit:30,
        order:"new",
        skip:0,
        first:1  //两个值,为0则不再请求图文
      },
      album:[],
      wallpaper:[],
      isHas:true
    }
  },
  //触底事件
  onReachBottom(){
    if(this.isHas){
    this.params.skip+=this.params.limit
    this.getlist();
    }else{
       uni.showToast({
              title:"到底了哦",
              icon:"none"
            })
    }

  }, 
  methods:{
    //获取数据
      getlist(){
        this.request({
          url:`http://157.122.54.189:9088/image/v1/wallpaper/album/${this.id}/wallpaper`,
          data:this.params
        }).then(res=>{
          if(Object.keys(this.album).length===0){
            this.params.first = 0;
          this.album = res.data.res.album
          }
          if(res.data.res.wallpaper.length===0){
             uni.showToast({
              title:"到底了哦",
              icon:"none"
            })
            this.isHas = false;
            return;
          }
          this.wallpaper = [...this.wallpaper,...res.data.res.wallpaper] 
        })
      }
  }


}
</script>

<style lang="scss" scoped>
.album_wrap {
  .album_item {
     position: relative;
    .album_title {
      padding:0 20rpx;
      width: 100%;
      display: flex;
      justify-content:space-between;
      align-items: center;
      position: absolute;
      left:0;
      bottom:30rpx;
      .album_name {
        color: #fff;
        font-size: 38rpx;
        opacity: 0.7;
      }
      .album_btn {
        margin-right: 10rpx;
        background-color:$color;
        padding: 10rpx 20rpx;
        color:#fff;
        border-radius: 10rpx;
      }
    }
  }
  .album_user {
    padding: 30rpx 20rpx;
    border-bottom: 1px solid #ccc;
    .user_head_img{
      display: flex;
      .user_image{
        image{
        border-radius: 50%;
        width: 90rpx;
        height: 90rpx;
        }
      }
      .user_name{
        color:#000;
        margin-left: 20rpx;
        font-size: 36rpx;
        font-weight: 600;
      }
    }
    .user_desc{
      margin-top: 20rpx;
      font-size: 32rpx;
    }
  }
}

  .wallpaper_wrap{
    display: flex;
    flex-wrap: wrap;
    .wallpaper_item{
      width: 33.33%;
      border: 3rpx solid #fff;
      image{
        height: 160rpx;
      }
    }
  }
</style>