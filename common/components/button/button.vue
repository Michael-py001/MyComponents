<template>
	<button class="c-button-warp lg" :class="[size, type,showShadow]" :style="customStyle" :disabled="disabled"
		:open-type="openType" @tap.stop="handleClick" @getphonenumber="getPhoneNumber" @getuserinfo="getUserInfo"
		@openSetting="openSetting">
		<slot>{{title}}</slot>
	</button>
</template>

<script>
	export default {
		props: {
			title: {
				type: null,
				default: '标题'
			},
			openType: {
				type: null,
				default: ''
			},
			size: {
				type: String,
				default: 'lg'
			},
			type: {
				type: String,
				default: 'primary'
			},
			disabled: {
				type: Boolean,
				default: false
			},
			customStyle: {
				default: ''
			},
			shadow: {
				default: false
			}
		},
		data() {
			return {

			};
		},
		computed: {
			showShadow() {
				return this.shadow ? '' : 'noShadow'
			}
		},
		methods: {
			handleClick() {
				if (!this.disabled) {
					this.$emit('click');
				} else {
					this.$emit('disabledClick');
				}
			},
			getUserInfo(e) {
				this.$emit('getuserinfo', e);
			},
			getPhoneNumber(e) {
				this.$emit('getphonenumber', e);
			},
			openSetting(e) {
				console.log('e', e)
				this.$emit('openSetting', e);
			}
		}
	}
</script>

<style lang="scss" scoped>
	.c-button-warp {
		box-sizing: border-box;
		width: 100%;
		background: #c4c4c4;
		color: #ffffff;

		&.noShadow {
			box-shadow: none !important;
		}

		&:active {
			opacity: .9;
		}

		// 主题
		&.primary {
			background-image: linear-gradient(270deg,
					#3c7ef8 0%,
					#14bfff 100%),
				linear-gradient(#c4c4c4,
					#c4c4c4);
			box-shadow: 0rpx 16rpx 16rpx 0rpx rgba(255, 48, 41, 0.20);
			color: #FFFFFF;
		}
    &.ghost {
      border: 2rpx solid #3c7ef8;
      color: #3c7ef8;
      background-color: #fff;
    }
		&.primaryTransparent {
			border: 1upx solid #c7defc;
			background: rgba(199, 222, 252, .5);
			font-size: 30upx !important;
			color: #FF6047;
		}

		&.cancel {
			background: #c4c4c4;
			color: #ffffff;
		}

		&.hollow {
			background-color: transparent;
			color: #333333;
			border: 2upx solid #e5e5e5;
		}

		&.hollowDdelete {
			color: #b2b2b2;
			background-color: transparent;
			border: none;
		}

		&.transparent {
			background-color: transparent;
			color: #b2b2b2;
			border: 2upx solid transparent;
		}

		&.disabled {
			opacity: 1;
			color: #ffffff;
		}

		&.disabledLogin {
			background: #c6cfdc;
			color: #ffffff;
		}

		&.primary-hollow {
			background-color: #ffffff;
			border: 2upx solid #FF6047;
			color: #FF6047;
		}

		&.primary-border {
			background-color: #F3F8FF;
			border: 2upx solid #c7defc;
			;
			color: #FF6047;
			font-weight: bold;
		}

		&.green-border {
			background-color: #EFFCF9;
			border: 2upx solid #a5e5d4;
			;
			color: #03CB95;
			font-weight: bold;
		}

		&.green {
			background-color: #dcf8f0;
			border: 2upx solid #dcf8f0;
			color: #03cb95;
			font-weight: bold;
		}

		&.LogOut {
			background-color: #F0F0F0;
			border: none;
			color: #888888;
		}

		&.default {
			background-color: #FFFFFF;
			border: 2upx solid #e5e5e5;
			color: #333333;
		}


		&.offLine {
			background: #f6f6f6;
			border: 2upx solid #e5e5e5;
			color: #b2b2b2;
		}

		// 大小
		&.lg {
      display: flex;
      justify-content: center;
      align-items: center;
			height: 80upx;
			line-height: 80upx;
			border-radius: 40upx;
			font-size: 32upx;
		}

		&.xl {
      display: flex;
      justify-content: center;
      align-items: center;
			height: 88upx;
			line-height: 86upx;
			border-radius: 44upx;
			font-size: 32upx;
		}
		
		&.xxl {
      display: flex;
      justify-content: center;
      align-items: center;
			height: 98upx;
			line-height: 96upx;
			border-radius: 48upx;
			font-size: 36upx;
		}

		&.x {
			height: 70upx;
			line-height: 70upx;
			border-radius: 35upx;
			font-size: 32upx;
		}

		&.md {
      display: flex;
      justify-content: center;
      align-items: center;
			height: 64upx;
			line-height: 64upx;
			border-radius: 32upx;
			font-size: 28upx;
		}

		&.sm {
      display: flex;
      justify-content: center;
      align-items: center;
			height: 50upx;
			line-height: 50upx;
			border-radius: 25upx;
			font-size: 26upx;
		}

		&.xs {
      display: flex;
      justify-content: center;
      align-items: center;
			height: 40upx;
			line-height: 40upx;
			border-radius: 20upx;
			font-size: 26upx;
		}

	}
</style>
