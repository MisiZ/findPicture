<template>
  <scroll-view scroll-y class="scroll_content" @scrolltolower="handleToLower" v-if="recommends.length>0">
    <!-- 推荐开始 -->
     <view class="recommend_wrap">
       <navigator class="recommend_item"
       v-for="item in recommends"
       :key="item.id"
       :url="`/pages/album/index?id=${item.id}`"
       >
         <image :src="item.thumb" mode="widthFix"></image>
       </navigator>
     </view>
     <!-- 推荐结束 -->
     <!-- 月份开始 -->
       <!-- 标题 -->
     <view class="mouth_wrap">
       <view class="mouth_title">
         <view class="mt_left">
           <view class="left_mouth">
             <text class="left_mouth1">{{monthes.DD}}/</text>
             <text class="left_mouth2">{{monthes.MM}}月</text>
           </view>
           <text class="left_txt">{{monthes.title}}</text>
         </view>
         <view class="mt_right">更多></view>
       </view>
         <!-- 图片内容 -->
       <view class="mouth_content">
         <view 
         class="content_item"
         v-for="(item,index) in monthes.items"
         :key="item.id"
         >
          <detail-view :list="monthes.items" :index="index">
           <image mode="aspectFill" :src="item.thumb+item.rule.replace('$<Height>',360)"></image>
          </detail-view>
         </view>
       </view>
     </view>
     <!-- 月份结束 -->
     <!-- 热门开始 -->
     <view class="hots_wrap">
       <view class="hots_title">
         <text>热门</text>
       </view>
       <view class="hots_content">
          <view  class="hots_item"
          v-for="(item,index) in hots"
          :key="item.id"
          >
            <detail-view :list="hots" :index="index">
              <image :src="item.thumb" mode="widthFix"></image>
            </detail-view>
          </view>
        </view>
     </view>
     <!-- 热门结束 -->
  </scroll-view>
</template>

<script>
import moment from "moment";
import detailView from "@/components/Detail" 
export default {
  data(){
    return{
    //推荐模块
    recommends:[],
    //月份模块
    monthes:[],
    //热门列表
    hots:[],
    params:{
      //获取多少条数据
      limit:30,
      //关键字
      order:'hot',
      //要跳过多少条数据
      skid: 0
    },
    hasMore:true
    }
  },
  components:{
     detailView
  },
   mounted(){
    uni.setNavigationBarTitle({
      title: '首页'
    });   
     this.getList()
   },
   methods:{
     //请求数据
     getList(){
        this.request({
            url:'http://157.122.54.189:9088/image/v3/homepage/vertical?limit=10&order=hot&skip=2',
            data:this.params
          }).then(res => {
            //判断一下是否第一次请求,避免多次请求
            if(this.recommends.length===0){
               //推荐模块
               this.recommends = res.data.res.homepage[1].items
               //月份模块
               this.monthes = res.data.res.homepage[2]
               this.monthes.MM = moment(this.monthes.stime).format("MM")
               this.monthes.DD = moment(this.monthes.stime).format("DD")
            }
            
            if(res.data.res.vertical===0){
               uni.showToast({
               title:"到底了哦",
               icon:"none"
               })
              this.hasMore=false;
              return;
            }
            //热门列表
            this.hots =[...this.hots,...res.data.res.vertical] 
          })
       },
     //滚动条触底事件
     handleToLower(){
       if(this.hasMore){
       //重新计算跳过多少条数据
       this.params.skid+=this.params.limit
       //请求数据
       this.getList()
       }else{
         uni.showToast({
           title:"没有图片了",
           icon:"none" 
         })
       }
     }
   }
}
</script>

<style lang="scss" scoped>
.scroll_content{
  height: calc(100vh - 36px);

  .recommend_wrap{
    display: flex;
    flex-wrap:wrap;
    .recommend_item{
       width: 50%;
       border: 3rpx solid #fff;
    }
  }

.mouth_wrap {
  .mouth_title {
    display: flex;
    justify-content: space-between;
    padding: 20rpx;
    .mt_left {
       color:$color;
       display: flex;
       font-weight: 600;
      .left_mouth {
       .left_mouth1 {
         font-size: 38rpx;
       }
      }
    }
    .left_txt{
       font-size: 38rpx;
       font-weight: 600;
       margin-left: 30rpx;
       color:#666;
      }

    .mt_right {
     color:$color;
     display: flex;
     justify-content: center;
    }
  }

  .mouth_content {
    display: flex;
    flex-wrap: wrap;
    .content_item{
      width: 33.33%;
      border: 3rpx solid #fff;
    }
  }
}

.hots_wrap{
  .hots_title{
    margin: 20rpx;
    padding-left: 10rpx;
    border-left: 5rpx solid $color;
    margin-left: 5rpx;
    font-size: 38rpx;
  }
.hots_content{
  display: flex;
  flex-wrap: wrap;
  .hots_item{
    width: 33.33%;
    border: 3rpx solid #fff;
  }
}
}
}
</style>