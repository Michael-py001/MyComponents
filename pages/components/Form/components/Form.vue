<template>
  <view class="form">
    <view class="form-form-input">
      <view class="label" :style="labelStyle">
        {{label}}
      </view>
      <input :type="type"  :disabled="disabled" :value="value" @input="onInput" :placeholder="holderPlaceText"  placeholder-class="form-form-input-input-class"/>
      <template v-if="mode=='getCode'">
        <view class="form-form-input-code" @click="getCode">
          {{isSendCode?countDown:'获取验证码'}}
        </view>
      </template>
    </view>
  </view>
</template>

<script>
  // <Form label="站点名称" labelWidth="230"  holderPlaceText="请输入站点名称" v-model="phone"></Form>
  // labelWidth 默认196rpx
  export default {
    props: {
      phone:{
        default:'' //getCode模式必传 校验手机号码
      },
      type:{ //input输入类型 string password
        default:'string'
      },
      mode:{
        default:'normal',//getCode 显示获取验证码 normal 普通输入
      },
      label:{ //label文字
        default:'label'
      },
      holderPlaceText:{ //holderPlace文字
        default:'请输入'
      },
      labelWidth:{ //label宽度
        default:196
      },
      disabled:{ //是否禁用input
        default:false
      }
    },
    data(){
      return {
        isSendCode:'',
        countDown:'',
        value:'',
        countDown:60,
        isSendCode:false,
      }
    },
    computed:{
      labelStyle(){
          return `width:${this.labelWidth}rpx;`
      }
    },
    methods: {
      getCode() {
        if(this.isSendCode) return
        if(!/^1\d{10}/.test(this.phone)) {
          this.$ShowToast('请输入正确的手机号')
          return
        }
        let data = {
          phone : this.phone
        }
        this.$Post('Login.getCode',data).then(res=>{
          this.$ShowToast('发送成功')
          this.isSendCode = true
          setInterval(()=>{
            this.countDown -=1
            if(this.countDown===0) {
              this.isSendCode = false
              this.countDown = 60
            }
          },1000)
        })
        
      },
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
        height: 100rpx;
        width: 100%;
        display: flex;
        // justify-content: space-between;
        align-items: center;
        border-bottom:  2rpx solid #E5E5E5;
        .label {
          width: 196rpx;
          text-align: left;
          color: #888888;
        }
        input {
          flex: 1;
        }
        &-input-class {
          font-size: 32rpx;
          font-weight: 55 Regular;
          text-align: left;
          color: #b2b2b2;
        }
        &-code {
          width: 170rpx;
          height: 60rpx;
          background: #073B84;
          border-radius: 32rpx;
          font-size: 26rpx;
          text-align: center;
          color: #fff;
          line-height: 60rpx;
        }
      }
      &-btn {
        margin-top: 80rpx;
      }
    }
  }
  
</style>
