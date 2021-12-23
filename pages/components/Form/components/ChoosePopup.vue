<template>
  <view class="form">
    <view class="form-form-input" :style="formInputStyle">
      <view class="label" :style="labelStyle">
        {{label}}
      </view>
      <view class="value" @click="popupShow=true" v-if="!noEditValue">
        <view class="name">
          {{value?value:'请选择'}}
        </view>
        <u-icon name="arrow-right" size="35"></u-icon>
      </view>
      <view class="noEditValue" :style="noEditValueStyle" v-else>
        {{noEditValue}}
      </view>
    </view>
    
    <u-select v-model="popupShow" :list="typeList" @confirm="confirm"></u-select>
  </view>
</template>

<script>
  // simple : <Form label="站点名称" labelWidth="230"  holderPlaceText="请输入站点名称" v-model="phone"></Form>
  export default {
    props: {
      typeList:{
        default:()=>([])
      },
      label:{
        default:'label'
      },
      labelWidth:{
        default:196
      },
      value:{
        default:'请选择'
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
        popupShow:false
      }
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
      confirm(value) {
        console.log(value[0].value)
        this.$emit('update:value',value[0].value)
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
        box-sizing: border-box;
        .label {
          width: 196rpx;
          text-align: left;
          color: #888888;
        }
        .value {
          display: flex;
          align-items: center;
          .name {
            font-size: 30rpx;
            color: #333333;
            margin-right: 18rpx;
          }
        }
        .noEditValue {
          color: #333333;
          font-size: 28rpx;
        }
      }
    }
  }
  
</style>
