<template>
  <view class="filter">
    <u-popup v-model="show" mode="bottom"   @close="show=false" border-radius="30" :closeable="true">
    			<view class="filter-title">筛选</view>
          <scroll-view :scroll-y="true" class="filter-scroll">
            <view class="filter-item" v-if="copySelectObj.orderType.length>0">
              <view class="filter-item-title">
                订单状态
              </view>
              <view class="filter-item-boxWrap">
                <view class="filter-item-boxWrap-box" :class="{'active':item.isSelect}" v-for="(item,index) in copySelectObj.orderType" :key="index" @click="clickOrderType('orderType',index,'order',item)">
                  {{item.label}}
                </view>
              </view>
            </view>
            <view class="filter-item" >
              <view class="filter-item-title">
                时间
              </view>
              <view class="filter-item-boxWrap">
               <MyCalendar @init="dataChange" @change="dataChange" ref="MyCalendar"></MyCalendar>
              </view>
            </view>
            <view class="filter-item" v-if="copySelectObj.deviceType.length>0">
              <view class="filter-item-title">
                设备型号
              </view>
              <view class="filter-item-boxWrap">
                <view class="filter-item-boxWrap-box" :class="{'active':item.isSelect}" v-for="(item,index) in copySelectObj.deviceType" :key="index" @click="clickOrderType('deviceType',index,'device',item)">
                  {{item.label}}
                </view>
              </view>
              
            </view>
            <view class="filter-item">
              <view class="filter-item-title">
                优惠券
              </view>
              <view class="filter-item-boxWrap">
                <view class="filter-item-boxWrap-box" :class="{'active':item.isSelect}" v-for="(item,index) in copySelectObj.couponType" :key="index" @click="clickOrderType('couponType',index,'coupon',item)">
                  {{item.label}}
                </view>
              </view>
              
            </view>
            <view class="filter-item" v-if="copySelectObj.packgeType.length>0">
              <view class="filter-item-title">
                套餐
              </view>
              <view class="filter-item-boxWrap">
                <view class="filter-item-boxWrap-box" :class="{'active':item.isSelect}" v-for="(item,index) in copySelectObj.packgeType" :key="index" @click="clickOrderType('packgeType',index,'packge',item)">
                  {{item.label}}
                </view>
              </view>
              
            </view>
          </scroll-view>
    			<view class="filter-btns">
    			  <view class="filter-btns-btn-left">
    			    <c-button type="disabled" @click="reset">重置</c-button>
    			    
    			  </view>
            <view class="filter-btns-btn-right">
              <c-button type="primary" @click="comfirm">确定</c-button>
            </view>
    			</view>
    </u-popup>
  </view>
</template>

<script>
  import MyCalendar from './MyCalendar.vue'
  export default {
    components:{
      MyCalendar
    },
    props: {
      mode:{
        default:'signal',//signal单选模式 many多选模式
      },
      selectValueObj:{
        type:Object,
        default:()=>({
          'order': [],
          'device': [],
          'coupon': [],
          'packge': []
        })
      },
      filterSelectObj:{
        type:Object,
        default:()=>({
          orderType:[],
          deviceType:[],
          couponType:[],
          packgeType:[]
        })
      },
      orderType:{
        type:Array,
        default:()=>([])
      },
      deviceType:{
        type:Array,
        default:()=>([])
      },
      couponType:{
        type:Array,
        default:()=>([])
      },
      packgeType:{
        type:Array,
        default:()=>([])
      },
	  couponTypeName:{
		  default:'优惠券类型'
	  }
    },
    data(){
      return {
        show:false,
        startDate:'',
        endDate:'',
        copySelectObj:{},
        copySelectValueObj:'',//深拷贝对象容器
        copyOrderType:'',//深拷贝对象容器
        copyDeviceType:'',//深拷贝对象容器
        copyCouponType:'',//深拷贝对象容器
        copyPackgeType:'',//深拷贝对象容器,
        couponList:[]
      }
    },
    watch: {
      show(value) {
        console.log("show:",value)
        if(!value) {
          console.log("关闭")
          this.$emit("hideFilter")
        }
      },
    },
    created() {
      uni.$on('init',()=>{
        this.copySelectValueObj = JSON.parse(JSON.stringify(this.selectValueObj))
        
        if(this.mode=='signal') {
          // 单选模式，把数组初始化成空字符串
          Object.keys(this.copySelectValueObj).forEach((item,index)=>this.copySelectValueObj[item]='')
          console.log("this.copySelectValueObj:",this.copySelectValueObj)
        }
        this.copySelectObj = JSON.parse(JSON.stringify(this.filterSelectObj)) 
        console.log('收到',this.copySelectObj)
      })
    },
    methods:{
      resetFilter() {
        for(let key in this.copySelectObj) {
          this.copySelectObj[key].forEach(item=>{item.isSelect=false})
        }
        // for (let i = 0; i < this.copyOrderType.length; i++) {
        //   this.copyOrderType[i].isSelect = false
        // }
        // for (let i = 0; i < this.copyDeviceType.length; i++) {
        //   this.copyDeviceType[i].isSelect = false
        // }
        // for (let i = 0; i < this.copyCouponType.length; i++) {
        //   this.copyCouponType[i].isSelect = false
        // }
        // for (let i = 0; i < this.copyPackgeType.length; i++) {
        //   this.copyPackgeType[i].isSelect = false
        // }
        for(let i in this.copySelectValueObj) {
          if(this.mode=='signal') {
            this.copySelectValueObj[i] = ''
          }
          else if(this.mode=='many') {
            this.copySelectValueObj[i].splice(0)
          }
        }
        console.log("copySelectValueObj:",this.copySelectValueObj)
      },
      dataChange({
        startDate = "",
        endDate = ""
      }) {
        this.startDate = startDate
        this.endDate = endDate
      },
      open() {
        this.show = true
      },
      clickOrderType(curName,itemIndex,from,item) {
        let curList = this.copySelectValueObj[from]
        if(this.mode=='many') {
          if(!curList.includes(item.value)) {
              this.$set(this.copySelectObj[curName][itemIndex],'isSelect',true)
              curList.push(item.value)
          }
          else if (curList.includes(item.value)) {
            let index = curList.indexOf(item.value)
            this.$set(this.copySelectObj[curName][itemIndex],'isSelect',false)
            this.copySelectValueObj[from].splice(index,1)
          }
        }
        else if(this.mode=='signal') {
          
          if(this.copySelectValueObj[from]) {
            // 如果是点击是同一个，设置false 清空值
            if(item.isSelect) {
              this.copySelectObj[curName][itemIndex].isSelect=false
              this.copySelectValueObj[from]=''
            }
            // 如果点击的是其他
            else {
              this.copySelectObj[curName].forEach((item,index)=>{
                item.isSelect = false
              })
              this.copySelectObj[curName][itemIndex].isSelect=true
              this.copySelectValueObj[from]=item.value
            }
          }
          // 如果没有设置值
          else {
            this.copySelectObj[curName].forEach((item,index)=>{
              item.isSelect = false
            })
            this.copySelectObj[curName][itemIndex].isSelect = true
            this.copySelectValueObj[from]=item.value
          }
        }
        
        console.log(this.copySelectValueObj)
      },
      comfirm(){
        let is_filter = false
        let is_selectObj = false
        for(let i in this.copySelectValueObj) {
          if(this.mode=='signal') {
            if(this.copySelectValueObj[i]) {
              is_selectObj = true
              break
            }
          }
          else if(this.mode=='many') {
            if(this.copySelectValueObj[i].length>0) {
              is_selectObj = true
              break
            }
          }
          
        }
        if((this.startDate&&this.endDate)||is_selectObj) {
          is_filter = true
        }
        console.log("is_filter:",is_filter)
        this.$emit('confirm',{startDate:this.startDate,endDate:this.endDate,selectObj:this.copySelectValueObj,is_filter:is_filter})
        this.show = false
      },
      reset() {
        console.log("重置")
        this.$refs.MyCalendar.reset()
        this.resetFilter()
        // this.$emit('reset')
      }
    },
    computed:{
    },
  }
</script>

<style lang="scss" scoped>
  .filter{
    &-title {
      font-size: 40rpx;
      font-weight: 600;
      text-align: center;
      color: #333333;
      line-height: 36rpx;
      padding: 30rpx 0;
      // position: sticky;
      top: 0;
    }
    &-scroll {
      min-height: 650rpx;
      max-height: 850rpx;
    }
    &-btns {
      width: 100%;
      height: 144rpx;
      background: #ffffff;
      display: flex;
      align-items: center;
      flex-direction: row;
      padding: 24rpx 30rpx;
      box-sizing: border-box;
      &-btn-left {
        width: 220rpx;
        margin-right: 20rpx;
      }
      &-btn-right {
        
        flex: 1;
      }
    }
    &-item {
      padding: 30rpx;
      &-title {
        font-size: 30rpx;
        font-weight: 600;
        text-align: left;
        color: #333333;
        line-height: 36rpx;
        margin-bottom: 24rpx;
      }
      &-boxWrap {
        width: 100%;
        display: flex;
        justify-content: flex-start;
        align-items: center;
        flex-direction: row;
        flex-wrap: wrap;
        &-box {
          // width: 214rpx;
          min-width: 30%;
          @include lineClamp();
          height: 72rpx;
          background: #f6f6f6;
          border-radius: 36rpx;
          font-size: 26rpx;
          font-weight: 55 Regular;
          text-align: center;
          color: #333333;
          line-height: 72rpx;
          margin-bottom: 14rpx;
          margin-right: 10rpx;
          padding: 0 20rpx;
          box-sizing: border-box;
          &.active {
            background: #f2f7ff;
            border: 1rpx solid #3c7ef8;
            border-radius: 37rpx;
            color: #3c7ef8;
          }
          // margin: 0 24rpx 12rpx 0;
        }
      }
      
    }
  }
</style>
