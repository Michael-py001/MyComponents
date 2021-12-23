<template>
  <view class="item">
    <PageEnd :type='loadingText'></PageEnd>
    <PageEnd :type='loadingText2'></PageEnd>
    <PageEnd :type='loadingText3'></PageEnd>
  </view>
</template>

<script>
  import PageEnd from './components/PageEnd.vue'
  export default {
    components:{
      PageEnd
    },
    data(){
      return {
        loadingText:'loading',
        loadingText2:'noMore',
        loadingText3:'noData'
      }
    },
    computed:{
    },
  }
</script>

<style lang="scss" scoped>
  .item{
    
  }
</style>
