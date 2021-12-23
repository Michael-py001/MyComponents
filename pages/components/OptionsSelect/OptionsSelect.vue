<template>
  <view class="item">
    <button class="btn" type="primary" @click="open">筛选-单选</button>
    <button class="btn" type="primary" @click="openFilter">筛选-多选</button>
    <filter @confirm="onFilterConfirm"  :filterSelectObj="filterSelectObj"
      @hideFilter="hideFilter" @openFilter="openFilter" ref="filter" :selectValueObj="filterObj" mode="many">
    </filter>
    <OptionsSelect :info="options" :show="show" @close="close" @change="onChange"></OptionsSelect>
  </view>
</template>

<script>
  import OptionsSelect from './components/li-OptionsSelect.vue'
  import filter from './components/filter.vue'
  export default {
    components: {
      OptionsSelect,
      filter
    },
    data() {
      return {
        show: false,
        options: [{
            title: '',
            list: [
              '今天',
              '正常',
              '停止',
              '异常'
            ],
            activeItem: 0
          },
          {
            title: '',
            list: [
              '最近创建',
              '消耗最高',
              '展示数最多',
              '点击数最多'
            ],
            activeItem: 0
          }, {
            title: '',
            list: [
              '营销目标',
              '投放状态',
              '计划状态',
            ],
            activeItem: 0
          },
          {
            title: '其他筛选',
            type: 'filter',
            list: [{
                title: '营销目标',
                list: [{
                    label: '不限',
                    value: 0
                  },
                  {
                    label: '直播带货',
                    value: 1
                  },
                  {
                    label: '短视频带货',
                    value: 2
                  },
                ],
                activeItem: 0
              },
              {
                title: '投放状态',
                list: [{
                    label: '不限',
                    value: 0
                  },
                  {
                    label: '已暂停',
                    value: 1
                  },
                  {
                    label: '投放中',
                    value: 2
                  },
                  {
                    label: '审核不通过',
                    value: 3
                  },
                ],
                activeItem: 0
              },
              {
                title: '计划状态',
                list: [{
                    label: '不限',
                    value: 0
                  },
                  {
                    label: '学习期',
                    value: 1
                  },
                  {
                    label: '成本保障期',
                    value: 2
                  },
                  {
                    label: '非学习期',
                    value: 3
                  },
                  {
                    label: '非成本保障期',
                    value: 4
                  },
                ],
                activeItem: 0
              },
            ],
          }
        ],
        // 筛选列表
        filterSelectObj: {
          orderType: [
            {
            "id": "",
            "name": "全部"
            },
            {
            "id": 1,
            "name": "待使用"
            },
            {
            "id": 2,
            "name": "已完成"
            },
            {
            "id": 3,
            "name": "已取消"
            }
          ],
          deviceType: [
            {
            "id": "5",
            "name": "测试机型"
            },
            {
            "id": "7",
            "name": "测试"
            },
            {
            "id": "8",
            "name": "GiveYouNature"
            }
          ],
          couponType: [
           {
           "id": "",
           "name": "全部"
           },
           {
           "id": 2,
           "name": "新客优惠券"
           },
           {
           "id": 0,
           "name": "无优惠券"
           },
           {
           "id": 1,
           "name": "普通优惠券"
           }
          ],
          packgeType: [
            {
            "id": "2",
            "name": "B套餐"
            },
            {
            "id": "3",
            "name": "C套餐"
            },
            {
            "id": "4",
            "name": "D套餐"
            },
            {
            "id": "5",
            "name": "立的套餐"
            },
            {
            "id": "6",
            "name": "999豪华套餐"
            },
            {
            "id": "7",
            "name": "买一送一尊享套餐999999"
            }
          ],
        },
        filterObj: {
          'order': [],
          'device': [],
          'coupon': [],
          'packge': []
        },
        filterShow: false,
      }
    },
    computed: {},
    onShow() {
      this.initFilter()
    },
    methods: {
      open() {
        this.show = true
        // this.$refs.MyCalendar.open()
      },
      close() {
        this.show = false
      },
      onChange(data) {
        console.log(data)
      },
      onFilterConfirm({
        startDate,
        endDate,
        selectObj,
       	is_filter
      }) {
        this.startDate = startDate
        this.endDate = endDate
        this.filterObj = selectObj
        this.is_filter = is_filter
        console.log("filterObj:", this.filterObj)
        console.log("Date", startDate, endDate)
        console.log("is_filter--", is_filter)
      },
      hideFilter() {
        this.filterShow = false
      },
      openFilter() {
        this.$refs.filter.open()
        this.filterShow = true
      },
      initFilter() {
      	const promise_one = this.getFilterOrderStatus
      	const promise_two = this.getFilterPackges
      	const promise_three = this.getFilterDeviceType
      	const promise_four = this.getCouponList
      	Promise.all([promise_one(), promise_two(), promise_three(), promise_four()]).then(res => {
      		console.log("promise-all", res)
      		uni.$emit('init')
      	}).catch(err => {
      		console.log("err-all:", err)
      	})
        // uni.$emit('init')
      },
      // 获取过滤器订单状态
      getFilterOrderStatus() {
      	return new Promise((resolve, reject) => {
          this.filterSelectObj.orderType = this.filterSelectObj.orderType.map((item) => {
          	return {
          		label: item.name,
          		value: item.id,
          		isSelect: false
          	}
          })
          resolve()
      	}).catch(err => {
      		reject(err)
      	})
      
      },
      // 获取过滤器套餐
      getFilterPackges() {
      	return new Promise((resolve, reject) => {
          this.filterSelectObj.packgeType = this.filterSelectObj.packgeType.map((item) => {
          	return {
          		label: item.name,
          		value: item.id,
          		isSelect: false
          	}
          })
          resolve()
      	}).catch(err => {
      		reject(err)
      	})
      
      },
      //获取过滤器设备型号
      getFilterDeviceType() {
      	return new Promise((resolve, reject) => {
      		this.filterSelectObj.deviceType = this.filterSelectObj.deviceType.map((item) => {
      			return {
      				label: item.name,
      				value: item.id,
      				isSelect: false
      			}
      		})
          resolve()
      	}).catch(err => {
      		reject(err)
      	})
      },
      getCouponList() {
      	return new Promise((resolve, reject) => {
          this.filterSelectObj.couponType = this.filterSelectObj.couponType.map((item) => {
          	return {
          		label: item.name,
          		value: item.id,
          		isSelect: false
          	}
          })
          resolve()
      	}).catch(err => {
      		reject(err)
      	})
      },
    }
  }
</script>

<style lang="scss" scoped>
  .item {
    .btn {
      margin: 20rpx 0;
    }
  }
</style>
