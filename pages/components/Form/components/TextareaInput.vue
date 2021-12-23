<template>
  <view class="form">
    <view class="form-form-input" :style="formInputStyle">
      <view class="label" :style="labelStyle">
        {{label}}
      </view>
      <view class="textarea-wrap" >
        <textarea class="textarea"  :disabled="disabled" v-model="innerValue" :placeholder="holderPlaceText" />
      </view>
    </view>
  </view>
</template>

<script>
  export default {
    props: {
      value:{
        default:''
      },
      label:{
        default:'label'
      },
      holderPlaceText:{
        default:'请输入'
      },
      labelWidth:{
        default:196
      },
      labelColor: {
        default: '#888888'
      },
      labelSize: {
        default: '28rpx'
      },
      disabled:{
        default:false
      },
      border:{
        default:true
      }
    },
    watch:{
      value(value){
        this.$emit('update:value',value)
        this.$emit('input',value)
      }
    },
    mounted() {
      if(this.value) {
        this.innerValue = this.value
      }
    },
    data(){
      return {
        innerValue:'',
      }
    },
    computed:{
      labelStyle() {
        return `width:${this.labelWidth}rpx;color:${this.labelColor};font-size:${this.labelSize}`
      },
      noEditValueStyle() {
        return `font-size:${this.labelSize};`
      },
      formInputStyle() {
        let str = ''
        if(!this.border) {
          str = `border-bottom:none;`
        }
        if (this.disabled) {
          str = `border-bottom:none;` //不可编辑不显示底部border
        } else {
          str = str + ``
        }
        return str
      }
    },
    methods: {
      onInput({detail:{value}}){
        this.$emit('input',value)
      }
    }
  }
</script>

<style lang="scss" scoped>
  .form{
    &-form {
      &-input {
        margin-bottom: 24rpx;
        height: 100%;
        width: 100%;
        display: flex;
        // justify-content: space-between;
        align-items: center;
        border-bottom:  1rpx solid #E5E5E5;
        .label {
          width: 196rpx;
          text-align: left;
          color: #888888;
        }
        .textarea-wrap {
          .textarea {
            background-color: transparent;
            width: 464rpx;
            height: 148rpx;
            border-radius: 10rpx;
            padding: 0 8rpx 8rpx;
            box-sizing: border-box;
            font-size: 26rpx;
            text-align: left;
            color: #000000;
            line-height: 47rpx;
          }
        }
      }
    }
  }
  
</style>
