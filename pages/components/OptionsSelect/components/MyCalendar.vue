<template>
	<view class="c-calendar-warp">
		<view class="c-calendar">
			<view class="item" @click="showCalendar=true">
        <text class="disabled" v-if="!startDate">选择开始时间</text>
				<text >{{startDate}}</text>
        <u-icon name="arrow-down" size="24rpx" color='#b2b2b2'></u-icon>
			</view>
			<text class="space"></text>
			<view class="item" @click="showCalendar=true">
        <text class="disabled" v-if="!endDate">选择结束时间</text>
				<text>{{endDate}}</text>
        <u-icon name="arrow-down" size="24rpx" color='#b2b2b2'></u-icon>
			</view>
		</view>
		<u-calendar :max-year="maxYear" :max-date="maxDate" v-model="showCalendar" :mode="mode" @change="change" @close="close">
		</u-calendar>
	</view>
</template>

<script>
	import dayjs from 'dayjs'
	export default {
		props: {
			mode: {
				default: 'range'
			}
		},
		data() {
			return {
				showCalendar: false,
				startDate: '',
				endDate: '',
				maxYear: '',
				maxDate: ''
			}
		},
		mounted() {
			this.initDate()
		},
		methods: {
      close(){
        this.showCalendar = false
      },
			change(e) {
        console.log(e)
				if (e) {
					this.startDate = e.endDate
					this.endDate = e.startDate
				}
				this.$emit('change',{
					startDate:this.startDate,
					endDate:this.endDate
				})
        this.close()
			},
      reset(){
        this.startDate = ''
        this.endDate = ''
      },
			// 初始化日期
			initDate() {
				let year = dayjs().year()
				let month = dayjs().month() + 1
				let startOfDay = dayjs().startOf('month').date()
				let endOfDay = dayjs().endOf('month').date()
				// this.startDate = `${year}-${month}-${startOfDay}`
				// this.endDate = `${year}-${month}-${endOfDay}`

				// 最大可切换年份
				this.maxYear = year
				// 最大可选日期
				this.maxDate = `${year}-${month}-${endOfDay}`
				this.$emit('init',{
					startDate:this.startDate,
					endDate:this.endDate
				})
			}
		}
	}
</script>

<style lang="scss">
	.c-calendar-warp {
		.c-calendar {
			display: flex;
			align-items: center;
			justify-content: center;
      flex-direction: row;
			.item {
				display: flex;
				align-items: center;
				justify-content: center;
        flex-direction: row;
				width: 242rpx;
				height: 66rpx;
				background-color: #f4f4f4;
				border-radius: 36rpx;
				font-size: 26rpx;
				color: #333333;
        text{
          margin-right: 4rpx;
        }
        .disabled {
          font-size: 26rpx;
          color: #b2b2b2;
          line-height: 32rpx;
          letter-spacing: -1rpx;
        }

				// &::after {
				// 	content: " ";
				// 	position: relative;
				// 	display: inline-block;
				// 	top: 4upx;
				// 	margin-left: 18upx;
				// 	width: 20rpx;
				// 	height: 20rpx;
				// 	// background-image: url("/static/images/icon/xia.png");
				// 	background-size: contain;
				// 	background-repeat: no-repeat;
				// }
			}

			.space {
        width: 146rpx;
        height: 2rpx;
        background: #e5e5e5;
				margin: 0 20upx;
				font-size: 26rpx;
				color: #333333;
			}
		}
	}
</style>
