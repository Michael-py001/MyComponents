<template>
  <view class="MoveCard">
    <TouchCard></TouchCard>
    <view class="wrap">
      <view class="">
        每次重置位置
      </view>
      <MyTouchCard></MyTouchCard>
      <view class="wrap">
      </view>
      <view class="">
        会记忆上次位置
      </view>
      <MyTouchCard2></MyTouchCard2>
    </view>
    <TouchCardNew></TouchCardNew>
  </view>
</template>

<script>
  import TouchCard from './components/touch-card-old.vue'
  import TouchCardNew from './components/touch-card.vue'
  import MyTouchCard from './components/my-move-card.vue'
  import MyTouchCard2 from './components/my-move-card-two.vue'
  export default {
    components:{
      TouchCard,
      MyTouchCard,
      TouchCardNew,
      MyTouchCard2
    },
    data(){
      return {
        
      }
    },
    computed:{
    },
  }
</script>

<style lang="scss" scoped>
  .MoveCard{
    .wrap {
      padding-top: 50rpx;
      padding-bottom: 250rpx;
    }
  }
</style>
