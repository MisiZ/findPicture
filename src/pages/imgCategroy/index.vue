<template>
    
     <view>
      <view class="home_tab">
          <view class="home_title">
            <uni-segmented-control
            :current="current"
            :values="items.map(v=>v.title)"
            @clickItem="onClickItem"
            style-type="text"
            active-color="#d4237a"
            ></uni-segmented-control>
          </view>
          <view class="iconfont icon-sousuo"></view>
       </view>

      <!-- <view class="content"> -->
        <!-- <view v-if="current === 0"></view> -->
        <!-- <view v-if="current === 1"></view> -->
      <!-- </view> -->
       <scroll-view scroll-y class="scroll_wrap" @scrolltolower="handleToLower">
      <view class="img_content">
        <view
        class="content_item"
        v-for="item in vertical"
        :key="item.id"
        >
          <image mode="aspectFill" :src="item.thumb"></image>
        </view>
      </view>
       </scroll-view>
     </view>
  
</template>

<script>
import { uniSegmentedControl } from "@dcloudio/uni-ui";

export default {
  data(){
    return{
      id:0,
      items: [
        {title:"最新",order:"new"},
        {title:"热门",order:"hot"}
      ],
      current: 0,
      params:{
        //获取多少数据
        limit:30,
        //跳过多少数据
        skip:0,
        //参数:new最新,hot热门
        order:"new" 
      },
      vertical:[],
      isHas:true
    }
  },
  components: {
    uniSegmentedControl
  },
  onLoad(options){
    this.id = options.id
    this.getList();
  },
  methods: {
    //获取数据
   getList(){
    this.request({
      url:`http://157.122.54.189:9088/image/v1/vertical/category/${this.id}/vertical`,
      data:this.params
       }).then(res=>{
         if(res.data.res.vertical.length===0){
           uni.showToast({
              title:"没有数据了",
              icon:"none"
           })
           this.isHas = false;
           return
         }
         this.vertical =[...this.vertical,...res.data.res.vertical] 
       })
    },
    //分段器点击事件
    onClickItem(e) {
      //改变索引
      if (this.current !== e.currentIndex) {
        this.current = e.currentIndex;
      }
      //通过索引改变order的值
      this.params.order = this.items[e.currentIndex].order
      this.params.skip = 0
      this.vertical = []
      //发送请求
      this.getList()
    },
    //滚动触底事件
    handleToLower(){
      if(this.isHas){
        this.params.skip+=this.params.limit;
        this.getList();
      }else{
         uni.showToast({
           title:"没有数据了",
           icon:"none"
         })
         return;
      }
    }
  }
}
</script>

<style scoped lang="scss">
  .home_tab{
    position: relative;
    .home_title{
      width: 60%;
      margin: 0 auto;
    }
    .icon-sousuo{
     position: absolute;
     top:50%;
     right:20rpx;
     transform: translateY(-50%);
    }
  }
  .scroll_wrap{
    height: calc(100vh - 36px);
.img_content{
   display: flex;
   flex-wrap: wrap;
   .content_item{
     width: 33.33%;
     border:3rpx solid #ffffff;
     image{
       height: 240rpx;
     }
   }
 }
  }
 

</style>