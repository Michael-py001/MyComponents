<template>
  <view class="AvatarList">
    <view class="item" v-for="(i,k) in iconList" :key="k" :style="iconStyle">
      <view class="icon-wrap">
        <image class="icon" :src="i" mode="aspectFill"></image>
      </view>
    </view>
  </view>
</template>

<script>
  export default {
    props:{
        iconList:{
          default:[]
        },
        gap:{
          default:55
        }
    },
    data(){
      return {
      }
    },
    computed:{
      iconStyle(){
        return `width:${this.gap}rpx;`
      }
    },
  }
</script>

<style lang="scss" scoped>
  .AvatarList{
    display: flex;
    padding: 24rpx;
    .item {
      position: relative;
      width: 55rpx;
      height: 70rpx;
      background-color: #ffff00;
      border: 1px solid #18B566;
      .icon-wrap {
        position: absolute;
        left: 0;
        top: 0;
        width: 72rpx;
        height: 72rpx;
        border-radius: 50%;
        overflow: hidden;
        .icon {
          width: 72rpx;
          height: 72rpx;
          border-radius: 50%;
        }
      }
    }
  }
</style>
