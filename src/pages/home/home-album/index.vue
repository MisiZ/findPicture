<template>
  <view>
      <scroll-view  scroll-y class="scroll_wrap" @scrolltolower="handleToLower">
       <!-- 轮播图开始 -->
        <view class="banner_wrap">
          <swiper indicator-dots autoplay circular>
            <swiper-item
            v-for="item in banner"
            :key="item.id"
            >
              <image :src="item.thumb"></image>
            </swiper-item>
          </swiper>
        </view>
    <!-- 轮播图结束 -->
    <!-- 列表开始 -->
        <view class="album_wrap">
          <navigator class="album_item"
           v-for="item in album"
           :key="item.id"
           :url="`/pages/album/index?id=${item.id}`"
           >
           <view class="album_img">
             <image :src="item.cover" mode="aspectFill"></image>
           </view>
           <view class="item_content">
             <view class="item_name">{{item.name}}</view>
             <view class="item_desc">{{item.desc}}</view>
             <view class="item_btn">
                <view class="btn_gz">+ 关注</view>
             </view>
           </view>
           </navigator>

        </view>
    <!-- 列表结束 -->
      </scroll-view>
    
  </view>
</template>

<script>
export default {
  data(){
    return{
        params:{
          limit:30,
          order:"new",
          skip:0
        },
        banner:[],
        album:[],
        isHave:true
    }
  },
  mounted(){
    uni.setNavigationBarTitle({
      title: '专辑'
    });   
    this.getList();
  },
  methods:{
    getList(){
      this.request({
        url:"http://157.122.54.189:9088/image/v1/wallpaper/album",
        data:this.params
      }).then(res=>{
        if(res.data.res.album.length===0){
           uni.showToast({
              title:"到底了哦",
              icon:"none"
            })
          this.isHave = false;
          return;
        }
        if(this.banner.length===0){
        this.banner = res.data.res.banner
        }
        this.album = [...this.album,...res.data.res.album]
      })
    },
    handleToLower(){
      if(this.isHave){
      this.params.skip+=this.params.limit
      this.getList()
      }else{
         uni.showToast({
           title:"到底了哦",
           icon:"none" 
         })
      }

    }
  }
}
</script>

<style lang="scss" scoped>
.scroll_wrap{
 height: calc(100vh - 36px);

.banner_wrap{
  swiper{
  height: calc(750rpx / 2.26);
    image{
      height: 100%;
    }
  }
}

.album_wrap {
  padding: 20rpx;
  .album_item {
    display: flex;
    border-bottom: 1px solid #ccc;
    .album_img{
       flex:1;
       padding: 20rpx;
      image{
       width: 200rpx;
       height: 200rpx;
      }
    }


    .item_content {
      padding: 0 10rpx;
      flex:2;
      overflow: hidden;
      .item_name {
        font-size: 34rpx;
        color: #000;
      }

      .item_desc {
       margin-top: 10rpx;
       font-size: 26rpx;
       overflow: hidden;
       white-space: nowrap;
       text-overflow: ellipsis;
      }

      .item_btn {
        padding:10rpx;
        margin-top: 50rpx;
        display: flex;
        justify-content: flex-end;

        .btn_gz{
          padding: 10rpx;
          border: 2rpx solid $color;
          color: $color;
        }
      }
    }
  }
}
}
</style>