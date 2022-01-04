<template>
  <view class="AvatarList">
    头像列表，常用于详情/点赞/评价
    <AvatarList :iconList="iconList" gap="55"></AvatarList>
    <AvatarList :iconList="iconList" gap="45"></AvatarList>
  </view>
</template>

<script>
  import AvatarList from './components/AvatarList.vue'
  export default {
    components:{
      AvatarList
    },
    data(){
      return {
        iconList:[
          '/static/AvatarList/avatar.png',
          '/static/AvatarList/avatar.png',
          '/static/AvatarList/avatar.png',
          '/static/AvatarList/avatar.png',
        ]
      }
    },
    computed:{
    },
  }
</script>

<style lang="scss" scoped>
  .AvatarList{
    display: flex;
    padding: 24rpx;
    flex-direction: column;
  }
</style>
