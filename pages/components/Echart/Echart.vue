<template>
	<u-grid :col="3">
		<u-grid-item @click="GoNav('Zhu_One')">
			<u-icon name="photo" :size="46"></u-icon>
			<view class="grid-text">柱状图-1</view>
		</u-grid-item>
		<u-grid-item @click="GoNav('Zhu_Two')">
			<u-icon name="lock" :size="46"></u-icon>
			<view class="grid-text">柱状图-2</view>
		</u-grid-item>
		<u-grid-item @click="GoNav('ZheXian_One')">
			<u-icon name="hourglass" :size="46"></u-icon>
			<view class="grid-text">折线图</view>
		</u-grid-item>
	</u-grid>
</template>

<script>
  export default {
    data(){
      return {
        
      }
    },
    methods: {
      GoNav(path) {
        if(path) {
          uni.navigateTo({
            url:`/pages/EchartDemo/pages/${path}/${path}`
          })
        }
        
      }
    }
  }
</script>

<style scoped lang="scss">
	.grid-text {
		font-size: 28rpx;
		margin-top: 4rpx;
		color: $u-type-info;
	}
</style>