<template>
	<view class="content">
		<view class="text-area" v-for="(item,index) in titles" :key="index"  @click="navTo(item.path)">
			<text class="title">{{item.title}}</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				titles: [
          {
            title:'自定义时间选择弹窗',
            path:'TimeSelect'
          },
          {
            title:'自定义下拉筛选',
            path:'OptionsSelect'
          },
          {
            title:'拖拽排序1',
            path:'Drag'
          },
          {
            title:'拖拽排序2',
            path:'Drag2' 
          },
          {
            title:'Echart图表',
            path:'Echart' 
          },
          {
            title:'表单',
            path:'Form' 
          },
          {
            title:'数据加载状态',
            path:'PageEnd' 
          },
          {
            title:'自定义顶部导航',
            path:'TopColumn' 
          },
        ]
			}
		},
		onLoad() {

		},
		methods: {
      navTo(path) {
        uni.navigateTo({
          url:`/pages/components/${path}/${path}`
        })
      }
		}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.text-area {
		display: flex;
		justify-content: center;
    width: 100%;
    padding: 15rpx;
    border-bottom: 1rpx solid #8f8f94;
     margin-bottom: 12rpx;
	}

	.title {
		font-size: 36rpx;
		color: #8f8f94;
   
	}
</style>
