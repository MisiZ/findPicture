<template>
    <view>
       <view class="video_tab">
          <view class="video_title">
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
        <view class="content">
          <view v-if="current<4"><video-main :urlObj={url:items[current].url,params:items[current].params}></video-main></view>
          <view v-if="current===4"><video-class :urlObj={url:items[current].url,params:items[current].params}></video-class></view>
        </view>
    </view>
</template>

<script>
import { uniSegmentedControl } from "@dcloudio/uni-ui";
import videoClass from "./videoclass/videoClass"
import videoMain from "./videomain/videoMain"

export default {
 data(){
    return{
      id:0,
      items: [
        {title:"推荐",url:"http://157.122.54.189:9088/videoimg/v1/videowp/featured",params:{limit:30,skip:0,order:"hot"}},
        {title:"娱乐",url:"http://157.122.54.189:9088/videoimg/v1/videowp/category/59b25abbe7bce76bc834198a",params:{limit:30,skip:0,order:"new"}},
        {title:"最新",url:"http://157.122.54.189:9088/videoimg/v1/videowp/videowp",params:{limit:30,skip:0,order:"new"}},
        {title:"热门",url:"http://157.122.54.189:9088/videoimg/v1/videowp/videowp",params:{limit:30,skip:0,order:"hot"}},
        {title:"分类",url:"http://157.122.54.189:9088/videoimg/v1/videowp/category",params:{}}
      ],
      current: 0
    }
  },
  components: {
    uniSegmentedControl,
    videoClass,
    videoMain
  },
  methods:{
    onClickItem(e) {
      //改变索引
       if (this.current !== e.currentIndex) {
         this.current = e.currentIndex;
       }
    }
  }
}
</script>

<style scoped lang="scss">

  .video_tab{
    position: relative;
    .video_title{
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
</style>