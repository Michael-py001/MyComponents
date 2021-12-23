<template>
  <view class="form">
    <view class="form-form-input">
      <view class="label" :style="labelStyle">
        {{label}}
      </view>
      <view class="boxs" v-for="(item,index) in checkList" :key="index" >
        <image src="/static/Form/checkboxs/nocheck.png" mode="aspectFill" v-if="!item.selected" @click="$emit('click',item)"></image>
        <image src="/static/Form/checkboxs/checked.png" mode="aspectFill" v-if="item.selected" @click="$emit('click',item)"></image>
        <view class="label2">
          {{item.label}}
        </view>
      </view>
    </view>
  </view>
</template>

<script>
  /**
   * exemple <Check label="类型" :checkList="checkList" @click="checkboxChange"></Check>
   * 
   * 传入数据格式
   * checkList:[
        {
          id:0,
          label:'站长',
          value:0,
          selected:true
        },
        {
          id:1,
          label:'快递员',
          value:1,
          selected:false
        },
      ],
      选中事件
      checkboxChange(checkbox){
        console.log(checkbox)
        this.checkList.forEach(item=>{
          if(item.id==checkbox.id) {
            item.selected = true
            this.selectCheckboxValue = checkbox.value
            console.log("value:",this.selectCheckboxValue)
          }
          else {
            item.selected = false
          }
        })
      }
   * */
  export default {
    props: {
      checkList:{ //数据格式
        default:()=>({})
      },
      label:{ //label文字
        default:'label'
      },
      labelWidth:{ //label宽度
        default:196
      },
      labelColor:{ //label颜色
        default:'#888888'
      },
      labelSize:{ //label字体大小
        default:'28rpx'
      },
    },
    data(){
      return {
        value:'',
        selectIndex:0,
      }
    },
    computed: {
      labelStyle(){
          return `width:${this.labelWidth}rpx;color:${this.labelColor};font-size:${this.labelSize}`
      },
    },
    methods: {
    }
  }
</script>

<style lang="scss" scoped>
  .form{
    &-form {
      
      &-desc {
        font-size: 24rpx;
        font-weight: 55 Regular;
        text-align: center;
        color: #b2b2b2;
        &-blue {
          color: #3C7EF8;
        }
      }
      &-input {
        margin-bottom: 24rpx;
        height: 100rpx;
        width: 100%;
        display: flex;
        align-items: center;
        border-bottom:  1rpx solid #E5E5E5;
        .label {
          width: 196rpx;
          text-align: left;
          color: #888888;
        }
        .boxs {
          display: flex;
          flex: 1;
          .label2 {
            text-align: left;
            color: #888888;
          }
          image {
            width: 32rpx;
            height: 32rpx;
            margin-right: 22rpx;
          }
        }
        
      }
    }
  }
  
</style>
