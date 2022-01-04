<template>
	<view class="touch-card-container" :class="{'show':show}">
		<view class="list" @touchstart="handletouchstart" @touchmove.stop.prevent="handletouchmove" @touchend="handletouchend"
		 @touchcancel="handletouchend">

			<block v-for="(i,k) in items" :key="k">
				<view class="item" :class="{'bg':k==items.length-1}" :style="k==items.length-1?activeItemStyle:i.style">
					<view class="item-view">
						<view class="text u-line-2">
							{{i.content}}
						</view>
					</view>
				</view>
			</block>
		</view>
	</view>
</template>

<script>
	export default {
		props: {
			ids: {
				default: ''
			}
		},
		data() {
			return {
				list: [
          {
            content: "汇聚精英人才顾问、资深文案、境内外律师，一次选择终身服务",
            id: 84,
          },
          {
            content: "汇聚精英人才顾问、资深文案、境内外律师，一次选择终身服务",
            id: 84,
          },
          {
            content: "汇聚精英人才顾问、资深文案、境内外律师，一次选择终身服务",
            id: 84,
          },
          {
            content: "汇聚精英人才顾问、资深文案、境内外律师，一次选择终身服务",
            id: 84,
          },
          {
            content: "汇聚精英人才顾问、资深文案、境内外律师，一次选择终身服务",
            id: 84,
          },
          
        ],
				// ------------
				flag: 0,
				text: '0',
				firstX: 0,
				lastX: 0,
				lastY: 0,
				lock: false,
				// 选中item的x轴偏移 
				offsetX: 0,
				item_style: '',
				// 移除按钮锁
				remove_btn_lock: false,
				// -------------
			};
		},
		watch: {
			ids(n) {
				this.infoAdvantage()
			}
		},
		mounted() {
			// this.infoAdvantage()
			console.log('mounted', this.ids)
		},
		methods: {
			// 服务优势
			infoAdvantage() {
				this.$Request('StudyAbroad.infoAdvantage', {
					id: this.ids
				}).then(res => {
					this.list = res.data
				})
			},
			remove() {
				if (this.remove_btn_lock) return
				this.remove_btn_lock = true
				let item = this.list.splice(-1, 1)
				setTimeout(() => {
					this.item_style = ''
					this.list.unshift(...item)
					this.remove_btn_lock = false
					this.lock = false
				}, 300)
			},
			handletouchstart(event) {
				this.firstX = event.touches[0].pageX;
				this.firstY = event.touches[0].pageY;
				this.lastX = event.touches[0].pageX;
				this.lastY = event.touches[0].pageY;
			},
			handletouchmove(event) {
				if (this.lock) return
				let currentX = event.touches[0].pageX;
				let currentY = event.touches[0].pageY;
				let tx = currentX - this.lastX;
				let ty = currentY - this.lastY;
				let text = '';
				//左右方向滑动
				if (Math.abs(tx) > Math.abs(ty)) {
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
						// 这里将向上归纳为向左
						this.flag = 1;
						//  this.getList();  //调用列表的方法
					} else if (ty > 0) {
						text = '向下滑动';
						// 这里将向上归纳为向右
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
				if (this.lock) return
				let difference = this.lastX - this.firstX
				// 滑动距离大于50
				if (Math.abs(difference) > 50 && this.list.length > 0) {
					this.lock = true
					this.distance = difference > 0 ? this.offsetX + difference + 750 : this.offsetX + difference - 750
					this.item_style = `transform: translateX(${this.distance}px);transition: 0.3s;`
					// 上面移除动画执行后，替换列表元素位置
					setTimeout(() => {
						this.remove()
					}, 200)
				}
			},
		},
		computed: {
			show() {
				return this.items && this.items.length > 0
			},
			items() {
				let lel = this.list.length
				let items = this.list.map((i, k) => {
					// 倒叙排序
					let n = lel - (k + 1)
					// 顶部距离
					let top = n <= 2 ? n * 20 : 2 * 20
					// 缩小区间
					let benchmark = 0.05
					// 缩放比例
					let scale = n <= 2 ? 1 - (benchmark * n) : benchmark * 2

					return {
						...i,
						scale,
						n,
						top,
						style: `top:${top}rpx;transform: scale(${scale});`
					}
				})
        console.log("items:",items)
				return items
			},
			activeItemStyle() {
				let item = this.items[this.items.length - 1]
				let style = item && item.style ? item.style : ''
				let difference = this.lastX - this.firstX
				if (!this.remove_btn_lock) {
					if (!this.lock) {
						let value = this.offsetX
						let y = 0
						let deg = 0
						if (this.flag !== 0) {
							value += difference
							y = difference < 0 ? value / 2 : -(value / 2)
							// (滑动距离/元素宽度一半)*45度
							deg = (Math.abs(difference) / 337) * 90
							deg = difference < 0 ? deg : -deg
						}
						style = `transform: rotate(${-deg}deg) translate(${value}px,${y}px);transition: 0s;transform-origin:50% 50%;`
					} else {
						style = this.item_style
					}
				}

				return style
			}
		}
	}
</script>

<style lang="scss">
	.touch-card-container {

		display: none;

		&.show {
			display: block;
		}

		.list {
			position: relative;
			margin-bottom: 24rpx;
			height: 170rpx;

			.item {
				position: absolute;
				left: 0;
				width: 100%;
				height: 124rpx;
				line-height: 124rpx;
				background-color: #2e8ff4;
				box-shadow: 0rpx 6rpx 10rpx 0rpx rgba(20, 22, 51, 0.3);
				border-radius: 16rpx;
				color: #FFFFFF;
				font-size: 24rpx;
				text-align: center;
				transition: .8s;

				&.bg {
					.item-view {
						&::after {
							z-index: 8;
							content: " ";
							position: absolute;
							top: 0;
							left: 0;
							width: 100%;
							height: 100%;
							background-image: url('/static/MoveCard/touch-card-bg.png');
							background-size: 710rpx 168rpx;
							background-repeat: no-repeat;
							background-position: -10rpx 0;
							border-radius: 16rpx;
							overflow: hidden;
						}
					}
				}

				.item-view {
					position: relative;
					padding-top: 18rpx;
					padding-left: 78rpx;
					height: 100%;

					.text {
						position: relative;
						z-index: 10;
						box-sizing: border-box;
						width: 550rpx;
						height: 92rpx;
						line-height: 46rpx;
						font-family: PingFang-SC-Medium;
						font-size: 28rpx;
						color: #ffffff;
					}
				}
			}
		}
	}
</style>
