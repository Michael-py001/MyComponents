<template>
	<view class="content">
		<!-- 参数 
		* @property {ObjectArray} list = [] 列表数据,自定义数据，会传递到name="rowContent"插槽  
		* @property {Boolean} feedbackGeneratorState = [true|false] 是否开启拖动触感反馈  
		* @property {Boolean} isLongTouch = [true|false] 是否开启长按拖动  
		* @property {Boolean} isAutoScroll = [true|false] 是否拖拽至边缘自动滚动列表  
		* @property {Number} longTouchTime = [] 选填,触发长按时长,单位:ms,默认350ms,不支持微信小程序 
		* @property {Number} listHeight = [] 选填,可拖动列表整体的高度,单位:px,默认等于窗口高度 
		* @property {Number} rowHeight = [] 必填,行高,单位:px,默认44px
		* @event {Function} change 行位置发生改变时触发事件 返回值:{index:'原始下标',moveTo:'被拖动到的下标',moveRow:'拖动行数据'}   
		* @event {Function} confirm 拖拽结束且行位置发生了改变触发事件 返回值:{index:'原始下标',moveTo:'被拖动到的下标',moveRow:'拖动行数据',list:'整个列表拖动后的数据'}  
		* @event {Function} onclick 点击行触发事件 返回值:{index:'被点击行下标',value:'被点击行数据'} 
		*/ 
		-->
		<!-- 当前属性设置 isLongTouch关闭长按拖动 isAutoScroll开启自动滚动 feedbackGeneratorState开启触感反馈 rowHeight行高55 -->
		<HM-dragSorts :list="list" :isLongTouch="false" :isAutoScroll="true" :feedbackGeneratorState="false"  :rowHeight="55" @change="change" @confirm="confirm" @onclick="onclick" >
			<template v-slot:rowContent="{row}">
				<view class="row">
					<image v-if="row.icon" class="icon" :src="row.icon"></image>
					<text class="text">{{row.name}}</text>
				</view>
			</template>
		</HM-dragSorts>	
		
	</view>
</template>
<script>
  // 官方插件地址 https://ext.dcloud.net.cn/plugin?id=1372
	export default {
		data() {
			return {
				/*
					0.数据结构说明 可以在结构中添加其他的数据，比如id,value等，不影响组件使用，拖动调整顺序后，组件会把传入的数据排序后原封不动在@confirm中返回。
					
					1.动态修改list数据，组件会响应变化，但拖动排序不会影响list，
					  所以，要特别注意，在动态修改list数据时候，由于组件内数据可能已经拖动重新排序过，页面的list顺序和组件内数据顺序不一致了，
					  最好在操作list时候，先和@confirm返回的值同步一下，或者直接操作@confirm返回的值，然后赋值给list。
					  
					n.最后，组件你下载了，代码你也看了，不给个五星好评，我可要打人了啊  (╬￣皿￣)=○  ヽ(#`Д´)ﾉ
				*/
       
				list: [
					{
						"name": "花呗", 
						"icon": "/static/img/1.png", 
					},
					{
						"name": "余额宝",
						"icon": "/static/img/2.png"
					},
					{
						"name": "账户余额",
						"icon": "/static/img/3.png"
					},
					{
						"name": "交通银行信用卡(0001)",
						"icon": "/static/img/4.png"
					},
					{
						"name": "中国建设银行信用卡(4401)",
						"icon": "/static/img/5.png"
					},
					{
						"name": "网商储蓄卡(7223)",
						"icon": "/static/img/6.png"
					}
					,
					{
						"name": "花呗", 
						"icon": "/static/img/1.png", 
					},
					{
						"name": "余额宝",
						"icon": "/static/img/2.png"
					},
					{
						"name": "账户余额",
						"icon": "/static/img/3.png"
					},
					{
						"name": "交通银行信用卡(0001)",
						"icon": "/static/img/4.png"
					},
					{
						"name": "中国建设银行信用卡(4401)",
						"icon": "/static/img/5.png"
					},
					{
						"name": "网商储蓄卡(7223)",
						"icon": "/static/img/6.png"
					},
					{
						"name": "花呗", 
						"icon": "/static/img/1.png", 
					},
					{
						"name": "余额宝",
						"icon": "/static/img/2.png"
					},
					{
						"name": "账户余额",
						"icon": "/static/img/3.png"
					},
					{
						"name": "交通银行信用卡(0001)",
						"icon": "/static/img/4.png"
					},
					{
						"name": "中国建设银行信用卡(4401)",
						"icon": "/static/img/5.png"
					},
					{
						"name": "网商储蓄卡(7223)",
						"icon": "/static/img/6.png"
					},
					{
						"name": "花呗", 
						"icon": "/static/img/1.png", 
					},
					{
						"name": "余额宝",
						"icon": "/static/img/2.png"
					},
					{
						"name": "账户余额",
						"icon": "/static/img/3.png"
					},
					{
						"name": "交通银行信用卡(0001)",
						"icon": "/static/img/4.png"
					},
					{
						"name": "中国建设银行信用卡(4401)",
						"icon": "/static/img/5.png"
					},
					{
						"name": "网商储蓄卡(7223)",
						"icon": "/static/img/6.png"
					},
					{
						"name": "花呗", 
						"icon": "/static/img/1.png", 
					},
					{
						"name": "余额宝",
						"icon": "/static/img/2.png"
					},
					{
						"name": "账户余额",
						"icon": "/static/img/3.png"
					},
					{
						"name": "交通银行信用卡(0001)",
						"icon": "/static/img/4.png"
					},
					{
						"name": "中国建设银行信用卡(4401)",
						"icon": "/static/img/5.png"
					},
					{
						"name": "网商储蓄卡(7223)",
						"icon": "/static/img/6.png"
					},
					{
						"name": "花呗", 
						"icon": "/static/img/1.png", 
					},
					{
						"name": "余额宝",
						"icon": "/static/img/2.png"
					},
					{
						"name": "账户余额",
						"icon": "/static/img/3.png"
					},
					{
						"name": "交通银行信用卡(0001)",
						"icon": "/static/img/4.png"
					},
					{
						"name": "中国建设银行信用卡(4401)",
						"icon": "/static/img/5.png"
					},
					{
						"name": "网商储蓄卡(7223)",
						"icon": "/static/img/6.png"
					}
				],
				ispush:false
				
			}
		},
		onLoad() {
			
		},
		methods: {
			onclick(e){
				console.log('=== onclick start ===');
				console.log("被点击行: " + JSON.stringify(e.value));
				console.log("被点击下标: " + JSON.stringify(e.index));
				console.log('=== onclick end ===');
			},
			change(e){
				console.log('=== change start ===');
				console.log("被拖动行: " + JSON.stringify(e.moveRow));
				console.log('原始下标：',e.index);
				console.log('移动到：',e.moveTo);
				console.log('=== change end ===');
			},
			confirm(e){
				console.log("this.list: ",this.list);
				console.log("e.list: ",e.list);
				console.log('=== confirm start ===');
				console.log("被拖动行: " + JSON.stringify(e.moveRow));
				console.log('原始下标：',e.index);
				console.log('移动到：',e.moveTo);
				console.log('=== confirm end ===');
			}
		}
	}
</script>
<style lang="scss" scoped>
	//scoped css只在当前页生效 不影响子组件
	page {
		background-color: #efeff4;
	}
	@media (prefers-color-scheme: dark){  
	    page {
	    	background-color: #000000;
	    } 
	}
	.content {
		.row{
			display: flex;
			flex-direction: row;
			align-items: center;
			.icon{
				width: 30px;
				height: 30px;
				border-radius: 6px;
				margin-right: 13px;
			}
			.text{
				font-size: 13px;
			}
		}
	}
	
</style>
