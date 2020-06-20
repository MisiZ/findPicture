<template>
  <view class="goods_wrap">
     <navigator
     class="goods_item"
     v-for="item in goodsList"
     :key="item.id"
     :url="`/pages/imgCategroy/index?id=${item.id}`"
     >
      <image :src="item.cover" mode="aspectFill"></image>
      <view class="goods_name">{{item.name}}</view>
     </navigator>
  </view>
</template>

<script>
export default {
  data(){
    return{
      goodsList:[]
    }
  },
    mounted(){
    uni.setNavigationBarTitle({
      title: '分类'
    });
    this.getList()
    },
    methods:{
      getList(){
        this.request({
          url:"http://157.122.54.189:9088/image/v1/vertical/category"
        }).then(res=>{
          this.goodsList = res.data.res.category
          console.log(this.goodsList);
        })
      }
    }
}
</script>

<style scoped lang="scss">
.goods_wrap {
    display: flex;
    flex-wrap:wrap;
  .goods_item {
    position: relative;
    width: 33.33%;
    border:5rpx solid #fff;
    image {
     height: 240rpx;
    }

    .goods_name {
    position: absolute;
    left: 10rpx;
    bottom:10rpx;
    color:#fff;
    font-size:32rpx;
    }
  }
}
</style>