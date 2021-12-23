<template>
  <view class="form">
    <view class="form-form-input" :style="formInputStyle">
      <view class="label" :style="labelStyle">
        {{label}}
      </view>
      <view class="value" @click="popupShow=true" v-if="!noEditValue"> 
        <u-number-box v-model="innerValue" @change="valChange" min="1"></u-number-box>
      </view>
      <view class="noEditValue" :style="noEditValueStyle" v-else>
        {{noEditValue}}
      </view>
    </view>
  </view>
</template>

<script>
  // simple : <Form label="站点名称" labelWidth="230"  holderPlaceText="请输入站点名称" v-model="phone"></Form>
  export default {
    props: {
      value:{
        default:1
      },
      label:{
        default:'label'
      },
      labelWidth:{
        default:196
      },
      labelColor:{
        default:'#888888'
      },
      labelSize:{
        default:'28rpx'
      },
      noEditValue:{ //直接显示value值模式
        default:''
      }
    },
    data(){
      return {
        innerValue:1
      }
    },
    mounted() {
      this.innerValue = this.value
    },
    computed:{
      labelStyle(){
          return `width:${this.labelWidth}rpx;color:${this.labelColor};font-size:${this.labelSize}`
      },
      noEditValueStyle() {
        return `font-size:${this.labelSize};`
      },
      formInputStyle() {
        let str = ''
        if(this.noEditValue) {
          str = `justify-content: flex-start;border-bottom:none;`//有默认值不显示底部border
        }
        else {
          str = `justify-content: space-between;`
        }
        return str
      }
      
    },
    methods: {
      valChange(value) {
        console.log(value)
        this.$emit('update:value',value.value)
      }
    }
  }
</script>

<style lang="scss" scoped>
  .form{
    &-form {
      &-input {
        margin-bottom: 24rpx;
        height: 100rpx;
        width: 100%;
        display: flex;
        justify-content: space-between;
        align-items: center;
        border-bottom:  1rpx solid #E5E5E5;
        .label {
          width: 196rpx;
          text-align: left;
          color: #888888;
        }
        .value {
        }
      }
    }
  }
  
</style>
