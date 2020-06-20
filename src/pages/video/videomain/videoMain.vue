<template>
  <scroll-view scroll-y enable-flex class="scroll_wrap" @scrolltolower="handleToLower">
    <view class="scroll_item"
    v-for="item in videoList"
    :key="item.id"
    @click="handleClick(item)"
    >
      <image mode="aspectFill" :src="item.img"></image>
    </view>
  </scroll-view>
</template>

<script>
export default {
   data(){
    return {
     videoList:[],
     isHas: true
    }
   },
   props: {
      urlObj: Object
   },
    mounted() {
      this.getList();
    },

    //监听发生变化
    watch: {
      urlObj() {
        this.videoList = []
        this.getList();
      }
    },
  methods: {
    //获取数据
    getList() {
      this.request({
        url: this.urlObj.url,
        data: this.urlObj.params
      }).then(res => {
        if(res.data.res.videowp.length===0){
          uni.showToast({
          title:"到底了哦",
          icon:"none"
          })
          this.isHas = false;
          return
        }
        this.videoList = [...this.videoList,...res.data.res.videowp]
      });
    },
    //滚动事件
    handleToLower(){
      if(this.isHas){
        this.urlObj.params.skip+=this.urlObj.params.limit;
        this.getList();
      }else{
        uni.showToast({
          title:"到底了哦",
          icon:"none"
        })
      }
    },
    //点击进入下载视频
    handleClick(item){
     //把数据存入全局缓存
      getApp().globalData.videoList = item
     //跳转页面
     uni.navigateTo({
       url: '/pages/videoPlay/index'
     });
       
    }
  }
};
</script>

<style scoped lang="scss">
.scroll_wrap{
  display: flex;
  flex-wrap: wrap;
  height: calc(100vh - 36px);
  .scroll_item{
    border:3rpx solid #ffffff;
    width: 33.33%;
    image{
      height: 240rpx;
    }
  }
}
</style>