<template>
  <view class="item">
    <TopColumn titleColor="#fff" :btnBackground="true"  :isNearTop="isNearTop" :statusBarHeight="statusBarHeight"
      :navHeight="navHeight" :gapHeight="gapHeight" :btnHeight="btnHeight" title="砍价拿红包" v-if="show==1">
    </TopColumn>
    
    <TopColumn titleColor="#fff" :btnBackground="false"  :isNearTop="isNearTop" :statusBarHeight="statusBarHeight"
      :navHeight="navHeight" :gapHeight="gapHeight" :btnHeight="btnHeight" title="砍价拿红包" v-if="show==2">
    </TopColumn>
    
    <TopColumn titleColor="#fff" :btnBackground="true" backgroundColor="#eecd0e"  :isNearTop="isNearTop" :statusBarHeight="statusBarHeight"
      :navHeight="navHeight" :gapHeight="gapHeight" :btnHeight="btnHeight" title="砍价拿红包" v-if="show==3">
    </TopColumn>
    <TopColumn titleColor="#fff" :btnBackground="true" backgroundColor="#eecd0e"  :isNearTop="isNearTop" :statusBarHeight="statusBarHeight" :showBack="false"
      :navHeight="navHeight" :gapHeight="gapHeight" :btnHeight="btnHeight" title="砍价拿红包" v-if="show==4">
    </TopColumn>
    <button type="primary" @click="change" class="btn">切换</button>
  </view>
</template>

<script>
  import NavHeight from '@/utils/Mixin/NavHeight'
  import TopColumn from './li-top-column/li-top-column.vue'
  export default {
    mixins:[NavHeight],
    components:{
      TopColumn
    },
    data(){
      return {
        show:1
      }
    },
    computed:{
    },
    methods:{
      change(){
        ++this.show
        if(this.show>=5) {
          this.show = 1
        }
      }
    }
  }
</script>

<style lang="scss" scoped>
  .item{
    background-color:#888;
    min-height: 150vh;
    .btn {
      position: absolute;
      left: 50%;
      top: 90%;
      
      transform: translate(-50%,-50%);
    }
  }
</style>
