<template>
  <view class="my-move-card-wrap">
    <view class="text">
      {{text}}
    </view>
    <view class="move-card" @touchstart="handletouchstart" @touchmove.stop.prevent="handletouchmove"
      @touchend="handletouchend" @touchcancel="handletouchend">
      <block v-for="(i,k) in items" :key="k">
        <view class="item" :class="{'active':k==items.length-1}" :style="k==items.length-1?activeItemStyle:i.style">
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
        text: '没有滑动',
        active: 1,
        list: [{
            name: '1广之旅海外咨询服务有限公司成立于1999年6月，是岭南集团成员“广之旅”旗下企业',
            id: 1,
          },
          {
            name: '2广之旅海外咨询服务有限公司成立于1999年6月，是岭南集团成员“广之旅”旗下企业',
            id: 2,
          },
          {
            name: '3广之旅海外咨询服务有限公司成立于1999年6月，是岭南集团成员“广之旅”旗下企业',
            id: 3,
          },
          {
            name: '3广之旅海外咨询服务有限公司成立于1999年6月，是岭南集团成员“广之旅”旗下企业',
            id: 3,
          },
          {
            name: '3广之旅海外咨询服务有限公司成立于1999年6月，是岭南集团成员“广之旅”旗下企业',
            id: 3,
          },
          // {
          //   name: '4广之旅海外咨询服务有限公司成立于1999年6月，是岭南集团成员“广之旅”旗下企业',
          //   id: 4,
          //   style:`top:${0}rpx;transform: scale(${1})`
          // }
        ],
        firstX: 0,
        firstY: 0,
        lastX: 0,
        lastY: 0,
        diffX: 0,
        diffY: 0,
        x: -50,
        lock: false,
        flag: 0,
        // 选中item的x轴偏移
        offsetX: 0,
        remove_btn_lock: false,
        item_style: ''

      }
    },
    computed: {
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
        console.log("items:", items)
        return items
      },
      activeItemStyle() {
        let item = this.items[this.items.length - 1]
        let style = item && item.style ? item.style : ''
        let difference = this.lastX - this.firstX
        if (!this.remove_btn_lock) {
          console.log(666)
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
            style = `transform: translateX(${value}px);transition: 0s;`
            // style = `transform: rotate(${-deg}deg) translate(${value}px,${y}px);transition: 0s;transform-origin:50% 50%;`
          } else {
            style = this.item_style
          }
        }

        return style

      }
    },
    methods: {
      handletouchstart(e) {
        this.firstX = e.touches[0].pageX;
        this.firstY = e.touches[0].pageY;
        this.lastX = e.touches[0].pageX;
        this.lastY = e.touches[0].pageY;

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
      handletouchmove(event) {
        if (this.lock) return
        let currentX = event.touches[0].pageX;
        let currentY = event.touches[0].pageY;
        let tx = currentX - this.lastX;
        let ty = currentY - this.lastY;
        this.diffX = currentX - this.firstX
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
      handletouchend(e) {
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
      }
    }
  }
</script>

<style lang="scss" scoped>
  .my-move-card-wrap {
    display: flex;
    justify-content: center;
    flex-direction: column;

    .text {
      padding-bottom: 80rpx;
    }

    .move-card {
      padding: 20rpx 0 60rpx;
      width: 100%;
      height: 124rpx;
      position: relative;
      // background-color: #18B566;

      .item {
        box-sizing: border-box;
        padding: 24rpx;
        position: absolute;
        width: 100%;
        height: 119rpx;
        background-color: #2e8ff5;
        border-radius: 16rpx;
        // z-index: 3;
        // top: 30rpx;
        top: 0;
        transition: .8s;
        color: #fff;
        display: flex;
        justify-content: center;
        align-items: center;
        overflow: hidden;
        box-shadow: 0rpx 6rpx 10rpx 0rpx rgba(20, 22, 51, 0.3);

        // left: 50%;
        // transform: translateX(-50%);
        .item-view {
          .text {
            z-index: 10;
            position: relative;
            left: 48rpx;
            padding: 0 48rpx 0 0;
            font-family: PingFang-SC-Medium;
            font-size: 28rpx;
            color: #ffffff;
            overflow: hidden;
          }
        }

        &.active {
          // position: relative;
          // z-index: 10;
          // box-shadow: 0rpx 6rpx 10rpx 0rpx rgba(20, 22, 51, 0.3);
          // // top: -35rpx;
          // // left: 114rpx;
          // // left: 50%;
          // // transform: translateX(-50%);
          // width: 100%;
          // height: 124rpx;

          &::after {
            z-index: 8;
            content: " ";
            position: absolute;
            top: 0rpx;
            left: -10rpx;

            width: 100%;
            height: 100%;
            background-image: url('/static/MoveCard/touch-card-bg.png');
            background-size: 105%;
            background-repeat: no-repeat;
            border-radius: 16rpx;
            overflow: hidden;
          }

        }
      }
    }

  }
</style>
