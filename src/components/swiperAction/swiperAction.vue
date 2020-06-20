<template>
<view @touchstart="handleTouchStrat" @touchend="handleTouchEnd">
  <slot></slot>
</view>  
</template>

<script>
export default {
  data(){
    return{
      startX:0,
      startY:0,
      oldTime:0
    }
  },
  methods:{
    handleTouchStrat(event){
     this.startX = event.changedTouches[0].clientX
     this.startY = event.changedTouches[0].clientY
     this.oldTime = Date.now()
     
    },
    handleTouchEnd(event){
     const endX = event.changedTouches[0].clientX
     const endY = event.changedTouches[0].clientY
     const newTime = Date.now()
     if(newTime - this.oldTime>2000){
      return
     }
     let direction = ""
     if(Math.abs(endX - this.startX)>10 && Math.abs(endY - this.startY)<10){
       direction=endX-this.startX>0?'left':'right'
       this.$emit("touchDirection",{direction})
     }else{
       return
     }
    }
  }
}
</script>

<style>

</style>