<template>
  <view class="item">
    <button type="primary" @click="open">自定义时间列表</button>
    <MyCalendar @comfirm="comfirm" ref="MyCalendar"></MyCalendar>
    <view class="">
      开始时间：{{startDate}}
      结束时间：{{endDate}}
    </view>
    <view class="line">
      常用日历选择
    </view>
    <DateCalendar @init="dataChange" @change="dataChange" ></DateCalendar>
  </view>
</template>

<script>
  import MyCalendar from './components/MyCalendar.vue'
  import DateCalendar from './components/DateCalendar.vue'
  export default {
    components: {
      MyCalendar,
      DateCalendar
    },
    data() {
      return {
        show: false,
        startDate:'',
        endDate:''
      }
    },
    computed: {},
    methods: {
      open() {
        this.$refs.MyCalendar.open()
      },
      close(){
        this.show = false
      },
      comfirm(date) {
        console.log(date)
        let {endDate,startDate} = date
        this.startDate = startDate
        this.endDate = endDate
      },
      dataChange(e){
        let {endDate,startDate} = e
        console.log(e)
      }
    }
  }
</script>

<style lang="scss" scoped>
  .item {
    .line {
      margin-top: 50rpx;
      font-size: 32rpx;
      font-weight: 550;
      margin-bottom: 50rpx;
    }
  }
</style>
