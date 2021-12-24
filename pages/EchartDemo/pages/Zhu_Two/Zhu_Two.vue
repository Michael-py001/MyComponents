<template>
  <view class="container">
    <ec-canvas id="mychart-dom-bar" canvas-id="mychart-bar" :ec="ec" ref="chart"></ec-canvas>
    <button type="primary" @click="getWaterWeek">数据更新</button>
  </view>
</template>

<script>
  import ecCanvas from 'common/components/ec-canvas/ec-canvas.vue'
  import * as echarts from 'common/components/ec-canvas/echarts';
  export default {
    components: {
      ecCanvas
    },
    data() {
      return {
        ec: {},
        list:[]
      }
    },
    computed: {},
    methods: {
      getWaterWeek() {
        this.list = [{
            "x": "周一",
            "y": Math.round(Math.random()*100) 
          },
          {
            "x": "周二",
            "y": Math.round(Math.random()*200)
          },
          {
            "x": "周三",
            "y": Math.round(Math.random()*350)
          },
          {
            "x": "周四",
            "y": Math.round(Math.random()*300)
          },
          {
            "x": "周五",
            "y": Math.round(Math.random()*450)
          },
          {
            "x": "周六",
            "y": Math.round(Math.random()*600)
          },
          {
            "x": "周日",
            "y": Math.round(Math.random()*800)
          }
        ]
        this.createChart()
      },
      createChart() {
       var option = {
         tooltip: {
           trigger: 'axis', //触发类型。坐标轴触发，主要在柱状图，折线图等会使用类目轴的图表中使用。
           axisPointer: {
             // 坐标轴指示器，坐标轴触发有效
             type: 'shadow' // 默认为直线，可选为：'line' | 'shadow'
           },
           confine: true //是否将 tooltip 框限制在图表的区域内。
         },
         color: ['#5abff6'],
         grid: {
           left: "18%", //x 偏移量
           top: "10%", // y 偏移量
           width: "74%", // 宽度
           height: "65%" // 高度
         },
         xAxis: {//x轴
           type: 'category',//类目轴，适用于离散的类目数据
           data: this.list.map(i => i.x),
         },
         yAxis: {//Y轴
           type: 'value',// 数值轴，适用于连续数据
           splitNumber: 5,
           minInterval: 100,
           axisLabel: {
             formatter: '{value}ml'
           }
         },
         series: [{
           label: {
             show: true,
             position: 'top'
           },
           data: this.list.map(i => i.y),
           type: 'bar',//柱状图
           markLine: {
             symbol: 'none',
             data: [{
               type: 'average',
               name: '平均值'
             }]
           }
         }]
       };
        this.$refs.chart.init(config => {
          console.log("config:", config)
          const {
            canvas
          } = config;
          const chart = echarts.init(canvas, null, config);

          canvas.setChart(chart);
          chart.setOption(option);
          // 需要把 chart 返回
          return chart;
        });
      }
    },
    onReady() {
      // 创建方式一 ec:{option} 传入配置数据 直接init
      // this.$refs.chart.init()

      //创建方式二 在函数里初始化 并传入配置数据
      this.getWaterWeek()
    }
  }
</script>

<style lang="scss" scoped>

  ec-canvas {
    width: 100%;
    height: 100%;
  }
  .container {
    position: absolute;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
  
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: space-between;
    box-sizing: border-box;
  } 
</style>
