<template>
  <scroll-view scroll-y enable-flex class="scroll_wrap">
    <view class="scroll_item"
    v-for="item in videoList"
    :key="item.id"
    >
      <image mode="aspectFill" :src="item.img"></image>
    </view>
  </scroll-view>
</template>

<script>
export default {
   data(){
    return {
     videoList:[]
    }
   },
   props: {
      urlObj: Object
   },
    mounted() {
      this.getList();
    },
  methods: {
    //获取数据
    getList() {
      this.request({
        url: this.urlObj.url
      }).then(res => {
        console.log(res);    //没有数据
        this.videoList = res.data.res.videowp
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
    width: 50%;
    image{
      height: 240rpx;
    }
  }
}
</style>