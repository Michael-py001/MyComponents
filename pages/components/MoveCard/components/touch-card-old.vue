<template>
	<view class="touch-card-warp">
    {{text}}
		<view class="touch-card" @touchstart="handletouchstart" @touchmove.stop.prevent="handletouchmove" @touchend="handletouchend"
		 @touchcancel="handletouchend">
			<block v-for="(i,k) in list" :key="k">
				<view class="item" :class="{'active':active==i.id}" :style="activeItemStyle" v-if="active==i.id">
					<view class="item-view">
						<view class="text u-line-2">
							{{i.name}}
						</view>
					</view>
				</view>
				<view class="item" :class="{'nextActive':nextItemId==i.id}" v-else>
					<view class="item-view">
						<view class="text u-line-2">
							{{i.name}}
						</view>
					</view>
				</view>
			</block>
		</view>
	</view>

</template>

<script>
	export default {
		data() {
			return {
				active: 1,
				list: [{
						name: '1广之旅海外咨询服务有限公司成立于1999年6月，是岭南集团成员“广之旅”旗下企业',
						id: 1
					},
					{
						name: '2广之旅海外咨询服务有限公司成立于1999年6月，是岭南集团成员“广之旅”旗下企业',
						id: 2
					},
					{
						name: '3广之旅海外咨询服务有限公司成立于1999年6月，是岭南集团成员“广之旅”旗下企业',
						id: 3
					},
					{
						name: '4广之旅海外咨询服务有限公司成立于1999年6月，是岭南集团成员“广之旅”旗下企业',
						id: 4
					}
				],
				// 下一个元素
				nextItemId: 2,
				flag: 0,
				text: '0',
				firstX: 0,
				lastX: 0,
				lastY: 0,
				lock: false,
				// 设置居中时的偏移
				x: -14,
				// 样式
				item_style: ''
			}
		},
		mounted() {

		},
		computed: {
			activeItemStyle() {
				let style = ''
				if (!this.lock) {
					let value = this.x
					if (this.flag !== 0) { //如果不是停止
						value += this.lastX - this.firstX
					}
					style = `transform: translateX(${value}px);transition: 0s;`
				} else {
					style = this.item_style //使用移动时的样式
				}

				return style
			}
		},
		methods: {
			handletouchstart(event) {
				this.firstX = event.touches[0].pageX;
				this.lastX = event.touches[0].pageX;
				this.lastY = event.touches[0].pageY;
			},
			handletouchmove(event) {
				if (this.lock) return
				let currentX = event.touches[0].pageX;
				let currentY = event.touches[0].pageY;
        // 当前位置 - 开始点击位置
				let tx = currentX - this.lastX;//水平位置
				let ty = currentY - this.lastY;//垂直位置
        
				let text = '';
				this.mindex = -1;
				//左右方向滑动
				if (Math.abs(tx) > Math.abs(ty)) {//如果向左
					if (tx < 0) {
						text = '向左滑动';
						this.flag = 1;
						//  this.getList();  //调用列表的方法
					} else if (tx > 0) {
						text = '向右滑动';
						this.flag = 2;
					}
				}
				//上下方向滑动
				else {
					if (ty < 0) {
						text = '向上滑动';
						this.flag = 1;
						//  this.getList();  //调用列表的方法
					} else if (ty > 0) {
						text = '向下滑动';
						this.flag = 2;
					}
				}

				//将当前坐标进行保存以进行下一次计算
				this.lastX = currentX;
				this.lastY = currentY;
				this.text = text;
			},
			handletouchend(event) {
				this.flag = 0;
				this.text = '没有滑动';
				this.removerItem()
			},
			removerItem() {
				let difference = this.lastX - this.firstX //上一次结束的X - 首次点击x
				if (Math.abs(difference) > 100) {//如果滑动超过100px
					this.lock = true
					// 当前距离
					this.distance = difference > 0 ? this.x + difference + 750 : this.x + difference - 750 //移出屏幕外
					this.item_style = `transform: translateX(${this.distance}px);transition: 0.8s;` //设置样式
					// 选择
					setTimeout(() => {
						this.item_style = `transform: translateX(${this.x}px);transition: 0.3s;`
						let lel = this.list.length
						// 当前item
						let index = this.list.findIndex(i => {
							return this.active == i.id //找到激活的哪个item的id，再找到这个item在数组中的index
						})
            // 如果激活的这个item不是最后一个,切换成下一个 index+1
						if (index < lel - 1) {
							this.active = this.list[index + 1].id
						} else {
              // 如果index超出list长度 重置为第一个
							this.active = this.list[0].id
						}

						// 下个item
						index = this.list.findIndex(i => {
							return this.active == i.id
						})
            // 同理 找到下一个item
						if (index < lel - 1) {
							this.nextItemId = this.list[index + 1].id
						} else {
							this.nextItemId = this.list[0] ? this.list[0].id : ''
						}

						this.lock = false
					}, 500)
				}
			}
		}
	}
</script>

<style lang="scss">
	.touch-card-warp {
		overflow: hidden;

		.touch-card {
			position: relative;
			margin: 0 auto;
			padding: 20rpx 0 60rpx;
			width: 592rpx;
			height: 159rpx;

			.item {
				position: absolute;
				z-index: 3;
				top: 20rpx;
				transition: .3s;

				&.nextActive {
					z-index: 4;
				}

				&.active {
					position: relative;
					z-index: 10;
					top: -20rpx;

					.item-view {
						width: 645rpx;
						height: 124rpx;
						box-shadow: 0rpx 6rpx 10rpx 0rpx rgba(20, 22, 51, 0.3);

						&::after {
							z-index: 8;
							content: " ";
							position: absolute;
							top: 0rpx;
							left: -10rpx;
							width: 100%;
							height: 100%;
							background-image: url('/static/MoveCard/touch-card-bg.png');
							background-size: 690rpx 148rpx;
							background-repeat: no-repeat;
							border-radius: 16rpx;
							overflow: hidden;
						}
					}
				}

				.item-view {
					position: relative;
					width: 592rpx;
					height: 119rpx;
					background-color: #2e8ff5;
					border-radius: 16rpx;
					overflow: hidden;

					.text {
						z-index: 10;
						position: relative;
						padding: 18rpx 30rpx 0 78rpx;
						width: 100%;
						height: 100%;
						line-height: 46rpx;
						font-family: PingFang-SC-Medium;
						font-size: 28rpx;
						color: #ffffff;
						overflow: hidden;
					}
				}
			}
		}

	}
</style>
