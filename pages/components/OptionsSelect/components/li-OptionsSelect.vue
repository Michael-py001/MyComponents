<template>
  <view class="li-OptionsSelect">
    <u-popup v-model="innershow" mode="bottom" border-radius="20" @close="$emit('close')">
      <view class="content-wrap">
        <view class="title-wrap">
          <view class="title" @click="selectTitle(index)" :class="{active:activeOption==index}"
            v-for="(data,index) in options" :key="index">
            <text>{{data.title}}</text>
            <u-icon name="arrow-down-fill" :color="activeOption==index?'#007AFF':'#999999'"></u-icon>
          </view>
        </view>
        <!-- 多状态筛选 -->
        <template v-if="options[activeOption].type=='filter'">
          <view class="content">
            <view class="item" v-for="(data,indexOuter) in options[activeOption].list" :key="index">
              <view class="options">
                <view class="title">
                  {{data.title}}
                </view>
                <view class="item-wrap">
                  <view class="item" :class="{active:options[activeOption].list[indexOuter].activeItem==index}"
                    @click="selectItem(options[activeOption].list[indexOuter].activeItem,index,indexOuter)"
                    v-for="(item,index) in data.list" :key="index">
                    {{item.label}}
                  </view>
                </view>
              </view>
            </view>
          </view>
        </template>
        <!-- 单一筛选 -->
        <template v-if="options[activeOption].type!='filter'">
          <view class="content">
            <view class="item" @click="selectItem(activeOption,index)"
              :class="{active:options[activeOption].activeItem==index}"
              v-for="(data,index) in options[activeOption].list" :key="index">
              <text>{{data}}</text>
              <u-icon name="checkmark" color='#007AFF' v-if="options[activeOption].activeItem==index"></u-icon>
            </view>
          </view>
        </template>
      </view>

    </u-popup>
  </view>
</template>

<script>
  export default {
    props: {
      show: {
        type: Boolean,
        default: false
      },
      info:{
        default:()=>{}
      }
    },
    data() {
      return {
        innershow: false,
        activeOption: 3,
        options: []
      }
    },
    mounted() {
      this.options = JSON.parse(JSON.stringify(this.info))
      this.init()
    },
    watch: {
      show(bool, oldbool) {
        console.log(bool, oldbool)
        if (bool != oldbool) {
          this.innershow = bool
        }
      }
    },
    methods: {
      init() {
        this.options.forEach(item => {
          if (item.type != 'filter') {
            item.title = item.list[0]
          }

        })
      },
      // 切换标题
      selectTitle(index) {
        this.activeOption = index
      },
      // 切换选择内容
      selectItem(activeOption, index, outerIndex) {
        const handleData = () => {
          let data = ''
          let filterIndex = this.options.findIndex(item => item.type == 'filter') //找出多项筛选的下标
          let value = this.options[filterIndex].list.map(item => {
            return {
              title: item.title,
              select: item.activeItem
            }
          })
          data = this.options.map(item => {
            // 区分单选和多项筛选
            if (item.type == 'filter') {
              return {
                label: item.title,
                value: value
              }
            } else {
              return {
                label: item.title,
                value: item.activeItem
              }
            }

          })
          return data
        }
        if (this.options[this.activeOption].type != 'filter') {
          this.options[this.activeOption].activeItem = index
          this.options[this.activeOption].title = this.options[this.activeOption].list[index]
          let data = handleData()
          this.$emit('change', data)
        } else if (this.options[this.activeOption].type == 'filter') {
          this.options[this.activeOption].list[outerIndex].activeItem = index
          let data = handleData()
          this.$emit('change', data)
        }

      }
    }
  }
</script>

<style lang="scss" scoped>
  .li-OptionsSelect {
    .content-wrap {
      min-height: 400rpx;

      .title-wrap {
        padding: 12rpx 24rpx;
        display: flex;
        justify-content: space-between;
        align-items: center;
        flex-direction: row;

        .title {
          display: flex;
          flex-direction: row;

          &.active {
            color: #007AFF;
          }
        }
      }

      .content {
        .item {
          padding: 12rpx 24rpx;
          border-bottom: 1px solid #999999;
          display: flex;
          flex-direction: row;
          align-items: center;
          justify-content: space-between;

          &.active {
            color: #007AFF;
          }

          .options {
            // background-color: #4CD964;
            width: 100%;

            .title {
              font-weight: 550;
              padding: 12rpx 0;
            }

            .item-wrap {
              display: flex;
              flex-flow: row wrap;
              justify-content: flex-start;

              .item {
                margin-right: 20rpx;
                margin-bottom: 20rpx;
                border-radius: 12rpx;
                border: 1rpx solid #333;
                color: #333;

                &.active {
                  border: 1rpx solid #007AFF;
                  color: #007AFF;
                }
              }
            }
          }
        }
      }
    }


  }
</style>
