<template>
	<view class="c-calendar-warp">
		<view class="c-calendar">
			<view class="item" @click="showCalendar=true">
        <text class="disabled" v-if="!startDate">开始时间</text>
				<text class="text">{{startDate}}</text>
        <!-- <u-icon name="arrow-down" size="28"></u-icon> -->
			</view>
      <text class="gap">至</text>
			<!-- <text class="space"></text> -->
			<view class="item" @click="showCalendar=true">
        <text class="disabled" v-if="!endDate">结束时间</text>
				<text class="text"> {{endDate}}</text>
        <!-- <u-icon name="arrow-down" size="28"></u-icon> -->
			</view>
		</view>
		<u-calendar :max-year="maxYear" :max-date="maxDate" v-model="showCalendar" :mode="mode" @change="change">
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
			change(e) {
				if (e.startDate && e.endDate) {
					this.startDate = e.startDate
					this.endDate = e.endDate
				}
				this.$emit('change',{
					startDate:this.startDate,
					endDate:this.endDate
				})
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

			.item {
				display: flex;
				align-items: center;
				justify-content: center;
				width: 242rpx;
				height: 66rpx;
				line-height: 66upx;
				background-color: #f4f4f4;
				border-radius: 36rpx;
				font-size: 26rpx;
				color: #333333;
        .disabled {
          font-size: 26rpx;
          color: #888888;
          line-height: 32rpx;
          letter-spacing: -1rpx;
        }
        .text {
          // margin-right: 18upx;
        }
			}
      .gap {
        margin: 0 76rpx;
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
