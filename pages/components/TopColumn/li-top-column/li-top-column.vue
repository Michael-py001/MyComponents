<template>
	<div class="custom-nav-warp" :style="customNavStyle">
    <template v-if="img">
     <div class="use-img">
       <image src="/static/images/components/top_column/top_img.png" mode="aspectFill"></image>
     </div>
    </template>
    <template v-if="!img">
      <div  class="btn-warp">
      	<div class="btn back" :style="btnStyle" @click="$nav('back')" v-if="showBack">
          <image src="/static/TopColumn/back.png" mode="aspectFill"></image>
      	</div>
      </div>
      <div class="label" :style="{'color':titleColor}" v-if="title">{{title}}</div>
    </template>
		
	</div>
</template>

<script>
	export default {
		components: {
		},
		data() {
			return {}
		},
		props: {
			showBack: {
				type: Boolean,
				default: true
			},
      img:{
        type: Boolean,
        default: false
      },
			titleColor: {
				type: String,
				default: "#ffffff"
			},
      // 父元素是否占位
      showPlace: {
      	default: true
      },
			title: null,//标题
			isNearTop: null,//配合NavHeight
			statusBarHeight: null,
			navHeight: null,
      gapHeight:{
        default:14
      },
      btnHeight:{
        default:45
      },
      backgroundColor:{//背景色
        type: String,
        default: ''
      },
			btnBackground: {
				type: Boolean,
				default: true
			},
			iColor: {
				type: String,
				default: '#fff'
			},
			transparent: {
				type: Boolean,
				default: false
			},
			isSearch: {
				type: Boolean,
				default: false
			},
			inputBoxBg: {
				type: String,
				default: '#f9f9f9'
			},
			inputBg: {
				type: String,
				default: '#ffffff'
			},
			disabled: {
				type: Boolean,
				default: false
			}
		},
		computed: {
			customNavStyle() {
				let bg = ''
				// #ifndef MP-WEIXIN
				let padding_top =  this.statusBarHeight+this.gapHeight/2 
				// #endif
				 // #ifdef MP-WEIXIN
				 let padding_top =  this.statusBarHeight+this.gapHeight/2
				 // #endif
				let is_fixed = 'position: fixed; top:0px;'
				if(this.isNearTop) {
				  bg = 'background-color:#FF4A34;'
				  padding_top = this.statusBarHeight + this.gapHeight/2
				}
				if(!this.isNearTop) {
				  // is_fixed = ''
          if(this.backgroundColor) {
            bg = `background-color:${this.backgroundColor};`
          }
          else {
            bg = 'background-color:transparent;'
          }
				   
				   
				}
        if (this.transparent) {
        	bg = 'background-color:transparent;'
        }
				// height:${this.navHeight}px;
				// return `height:${this.navHeight}px;${bg}`
        console.log("this.gapHeight---:",this.gapHeight)
        // #ifndef MP-WEIXIN
        return `padding-bottom:${this.gapHeight*2}px; padding-top:${padding_top}px;height:${this.navHeight}px;${bg};${is_fixed}`
         // #endif
         
         // #ifdef MP-WEIXIN
         return `padding-bottom:${this.gapHeight}px; padding-top:${padding_top}px;height:${this.navHeight+this.gapHeight/2}px;${bg};${is_fixed}`
         // #endif
        // return `padding-bottom:${this.gapHeight/2}px; padding-top:${padding_top}px;height:${this.navHeight}upx;${bg};${is_fixed}`
			},
			btnStyle() {
				if (this.btnBackground) {
          if(this.backgroundColor) {
            return `background-color: transparent;`
          }
          else {
            return `background-color: rgba(0, 0, 0, 0.4);` // 圆圈返回按钮
          }
					
				} else {
					return `background-color:${this.backgroundColor};`
				}
			},
			iconStyle() {
				return `color: ${this.iColor};`
			}
		},
		methods: {
			handleSearch(e) {
				this.$emit('search', e)
			},
      $nav() {
        uni.navigateBack({
          delta:1
        })
      }
		}
	}
</script>


<style lang="scss" scoped>
	.custom-nav-warp {
		display: flex;
		align-items: center;
		z-index: 100;
		// position: fixed;
		// top: 0;
		// left: 0;
		box-sizing: border-box;
		padding-left: 14rpx;
		width: 100%;
		transition: 0.3s;
    .use-img {
      image{
        width: 487px;
        height: 52px;
      }
    }
		.btn-warp {
			position: relative;
			display: flex;
			height: 64rpx;
      width: 64rpx;
			border-radius: 32rpx;
      // margin-top: 10rpx;
			.btn {
				display: flex;
				align-items: center;
				justify-content: center;
				width: 64rpx;
				height: 64rpx;

				&.back {
					border-radius: 50%;
          image {
            width: 36rpx;
            height: 36rpx;
          }
				}

				&.home {
					border-radius: 0 32rpx 32rpx 0;
				}

				.iconfont {
					font-size: 36upx;
					color: #ffffff;
				}

				.icon {
					width: 36rpx;
					height: 36rpx;
				}
			}

		}

		.label {
			color: #fff;
			font-size: 34rpx;
			margin-left: 208rpx;
		}

		.search-box {}
	}
</style>
