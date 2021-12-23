<template>
  <view class="form">
    <view class="form-form-input" :style="formInputStyle">
      <view class="label" :style="labelStyle">
        {{label}}
      </view>
      <view class="imgs" v-if="!noEditValue">
        <template v-if="imgList.length>0">
          <view class="img-wrap" v-for="(item,index) in imgList" :key="index">
            <image class="img" :src="item" mode="aspectFill" @click.stop="previewImg(index,imgList)"></image>
            <image src="/static/Form/ChooseImages/close.png" mode="aspectFill" class="delete" @click="deleteImg">
            </image>
          </view>
          <image class="img" src="/static/Form/ChooseImages/add.png" mode="aspectFill" @click="chooseImg"
            v-if="imgList.length<limitNum"></image>
        </template>
        <template v-if="imgList.length==0">
          <view class="img-wrap">
            <image class="img" src="/static/Form/ChooseImages/add.png" mode="aspectFill" @click="chooseImg"></image>
          </view>

        </template>
      </view>
      <view class="imgs" v-if="noEditValue">
        <view class="img-wrap" v-for="(item,index) in noEditValue" :key="index">
          <image class="img" :src="item" mode="aspectFill" @click.stop="previewImg(index,noEditValue)"></image>
        </view>

      </view>
    </view>
  </view>
</template>

<script>
  /**
   * <ChooseImages label="站点图片" labelWidth="230" :imgList.sync="imgList"></ChooseImages>
   * 
   * imgList:[],//图片
   * 
   * */
  export default {
    props: {
      label: {
        default: 'label'
      },
      labelWidth: {
        default: 196
      },
      limitNum: {
        default: 3
      },
      labelColor: {
        default: '#888888'
      },
      labelSize: {
        default: '28rpx'
      },
      noEditValue: { //直接显示value值模式
        default: ''
      }
    },
    data() {
      return {
        imgList: []
      }
    },
    computed: {
      labelStyle() {
        return `width:${this.labelWidth}rpx;color:${this.labelColor};font-size:${this.labelSize}`
      },
      noEditValueStyle() {
        return `font-size:${this.labelSize};`
      },
      formInputStyle() {
        let str = ''
        if (this.noEditValue) {
          str = `border-bottom:none;` //有默认值不显示底部border
        } else {
          str = ``
        }
        return str
      }
    },
    methods: {
      deleteImg(index) {
        this.imgList.splice(index, 1)
        this.$emit('update:imgList', this.imgList)
      },
      // 预览图片
      previewImg(index, imgList) {
        let that = this
        uni.previewImage({
          current: index,
          urls: imgList,
          success: (res) => {
            console.log(res)
          },
          fail: (res) => {

          }
        })
      },
      upLoadImg(url) {
        return new Promise((resolve, reject) => {
          this.$Request("User.UploadImage", {
            file: url,
            fileName: "iFile",
          }).then(res => {
            resolve(res.data.file_img_url)
          }).catch(err => {
            reject(err)
          })
        })
      },
      ChooseImage({
        count = 9,
        sizeType = ['original', 'compressed'],
        sourceType = ['album', 'camera']
      } = {}){
        return new Promise((resolve, reject) => {
          uni.chooseImage({
            count,
            sizeType,
            sourceType,
            success: res => {
              resolve(res)
            },
            fail: res => {
              reject(res)
            }
          })
        })
      },
      chooseImg(index) {

        this.ChooseImage({
          count: this.limitNum
        }).then(async res => {
          let tempUrl = res.tempFilePaths
          for (let i = 0; i < res.tempFilePaths.length; i++) {
            let url = await this.upLoadImg(tempUrl[i])
            this.imgList.push(url)
          }
          if (this.imgList.length > this.limitNum) {
            this.imgList.splice(this.limitNum)
            this.$ShowToast('超出数量限制')
          }
          this.$emit('update:imgList', this.imgList)
          console.log("imgList:", this.imgList)
        })
      }
    }
  }
</script>

<style lang="scss" scoped>
  .form {
    &-form {
      &-input {
        margin-bottom: 24rpx;
        width: 100%;
        display: flex;
        align-items: center;
        border-bottom: 1rpx solid #E5E5E5;
        .label {
          width: 196rpx;
          text-align: left;
          color: #888888;
        }

        .imgs {
          .img {
            width: 140rpx;
            height: 140rpx;
          }

          .img-wrap {
            display: inline-block;
            box-sizing: border-box;
            text-align: left;
            position: relative;
            margin: 0 15rpx 24rpx 0;

            .img {
              width: 140rpx;
              height: 140rpx;

            }

            .delete {
              position: absolute;
              top: 10rpx;
              right: 10rpx;
              content: "";
              width: 40rpx;
              height: 40rpx;
            }
          }

        }
      }
    }
  }
</style>
