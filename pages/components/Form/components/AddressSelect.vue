<template>
  <view class="form">
    <view class="form-form-input">
      <view class="label" :style="labelStyle">
        {{label}}
      </view>
      <view class="content" @click="openLocation">
        <input :type="type"  :disabled="disabled" :value="address" @input="onInput" :placeholder="holderPlaceText"  placeholder-class="form-form-input-input-class"/>
        <u-icon name="map" size="34" color="#FF8F4E"></u-icon>
      </view>
    </view>
  </view>
</template>

<script>
  /**
   * <AddressSelect label="详细地址" labelWidth="230" disabled holderPlaceText="请选择详细地址" @comfirm="comfirmAddress"></AddressSelect>
   * 
   * comfirmAddress(address){
        console.log("address:",address)
        this.addressDetail = address
      },
   * 
   * */
  export default {
    props: {
      phone:{
        default:''
      },
      type:{
        default:'string'
      },
      mode:{
        default:'normal',//getCode 获取验证码
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
      disabled:{
        default:false
      }
    },
    data(){
      return {
        address:''
      }
    },
    computed:{
      labelStyle(){
          return `width:${this.labelWidth}rpx;`
      }
    },
    methods: {
      openLocation(){
        let _this = this
        uni.getLocation({
        	type: 'gcj02', //返回可以用于uni.openLocation的经纬度
          altitude:true,//高精度
        	success: function(res) {
        		const latitude = res.latitude;
        		const longitude = res.longitude;
        		uni.chooseLocation({
              latitude:latitude,
              longitude:longitude,
              success: function (res) {
                  console.log('位置名称：' + res.name);
                  console.log('详细地址：' + res.address);
                  console.log('纬度：' + res.latitude);
                  console.log('经度：' + res.longitude);
                  _this.$emit('comfirm',res.address)
                  _this.address = res.address
              }
        		});
        	}
        });
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
        .content {
          flex: 1;
          display: flex;
          input {
            flex: 1;
          }
          &-input-class {
            font-size: 32rpx;
            font-weight: 55 Regular;
            text-align: left;
            color: #b2b2b2;
          }
        }
       
      }
    }
  }
  
</style>
