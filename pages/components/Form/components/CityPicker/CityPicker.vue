<template>
  <view class="form">
    <view class="form-form-input">
      <view class="label" :style="labelStyle">
        {{label}}
      </view>
      <input :disabled="disabled" :value="cityName" @input="onInput" :placeholder="holderPlaceText"  placeholder-class="form-form-input-input-class" :placeholder-style="placeholderStyle" @click="pickerShow=true"/>
      <u-icon name="arrow-right" size="24"></u-icon>
    </view>
    <CityPicker :shake="shake" :area-code='areaCode' mode="region" v-model="pickerShow" title="城市地区" @confirm="confirmCity"></CityPicker>
  </view>
</template>

<script>
  /**
   *  <CityPicker label="站点地址" labelWidth="230" holderPlaceText="请选择城市地区" @comfirm="comfirmCity"></CityPicker>
   * 
   * 对外输出一个省市区数组
   * comfirmCity(arr) { 
        this.areaArr = arr
      },
   * 
   * */
  import CityPicker from './u-picker.vue'
  export default {
    components:{
      CityPicker
    },
    props: {
      placeHoderTextColor:{ //placeHoder颜色
        // default:'#b2b2b2'
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
      disabled:{ //禁用input
        default:true
      },
      labelColor:{
        default:'#888888'
      },
      labelSize:{
        default:'28rpx'
      },
      shake:{
        default:true //切换震动开关
      }
    },
    data(){
      return {
        pickerShow:false,
        province:'',
        area:'',
        city:'',
        areaCode:[]
      }
    },
    computed:{
      placeholderStyle(){
        return `color:${this.placeHoderTextColor};`
      },
      cityName(){
        if(!this.province) return ''
        return `${this.province}-${this.city}-${this.area}`
      },
      labelStyle(){
          return `width:${this.labelWidth}rpx;color:${this.labelColor};font-size:${this.labelSize}`
      },
      noEditValueStyle() {
        return `font-size:${this.labelSize};`
      },
      formInputStyle() {
        if(this.noEditValue) {
          return `justify-content: flex-start;`
        }
        else {
          return `justify-content: space-between;`
        }
      }
    },
    methods: {
      confirmCity(e) {
        console.log(e)
        let {area,city,province} = e
        this.area = area.label
        this.city = city.label
        this.province = province.label
        this.areaCode = [province.value,city.value,area.value]
        console.log("[this.province,this.area,this.city]",[this.province,this.area,this.city])
        this.$emit('comfirm',[this.province,this.city,this.area])
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
