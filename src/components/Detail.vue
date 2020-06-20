<template>
  <view @click="handleClick">
    <slot></slot>
  </view>
</template>

<script>
export default {
  props: {
    list: Array,
    index: Number
  },
  methods: {
    handleClick() {
      //缓存所有图片和索引
      getApp().globalData.imgList = this.list;
      getApp().globalData.imgIndex = this.index;
      //跳转到详情页面
      uni.navigateTo({
        url: "/pages/imgDetail/index"
      });
    }
  }
};
</script>

<style>
</style>