<!-- 横向柱状图 -->
<template>
  <div>
    <div class="text-lg">【大区数据信息】</div>
    <div ref="target" class="w-full h-full"></div>
  </div>
</template>

<script setup>
import { ref, onMounted, watch } from 'vue'
import * as echarts from 'echarts'

const props = defineProps({
  data: {
    typeof: Object,
    require: true,
  },
})
const target = ref(null)
let myChart = null

onMounted(() => {
  console.log(props.data.regions)
  myChart = echarts.init(target.value)
  renderChart()
})

let colorList = [
  'rgba(255, 174, 0, 1)',
  'rgba(183, 206, 255, 1)',
  'rgba(226, 134, 88, 1)',
  '#2379FF',
  '#2379FF',
  '#2379FF',
]

const randomRGB = () => {
  const R = Math.floor(Math.random() * 255)
  const G = Math.floor(Math.random() * 255)
  const B = Math.floor(Math.random() * 255)
  return `rgb(${R},${G},${B})`
}

const renderChart = () => {
  const options = {
    xAxis: {
      show: false,
      type: 'value',
      max: function (value) {
        return parseInt(value.max * 1.2)
      },
    },
    yAxis: {
      type: 'category',
      data: props.data.regions.map((item) => item.name),
      inverse: true,
      axisLine: {
        show: false,
      },
      axisTick: {
        show: false,
      },
      axisLabel: {
        color: '#fff',
        fontSize: 16,
        fontWeight: 'bold',
      },
    },
    tooltip: {
      //提示框
      trigger: 'item', //触发类型
    },
    grid: {
      top: 10,
      left: 0,
      right: 0,
      bottom: 10,
      containLabel: true,
    },
    series: [
      {
        type: 'bar',
        data: props.data.regions.map((item, index) => ({
          name: item.name,
          value: item.value,
          itemStyle: {
            borderRadius: [0, 30, 30, 0],
            borderColor: '#fff',
            color: {
              type: 'linear',
              x: 0,
              y: 0,
              x2: 1,
              y2: 1,
              colorStops: [
                {
                  offset: 0,
                  color: '#0F1F45',
                },
                {
                  offset: 1,
                  color: randomRGB(),
                },
              ],
            },
          },
        })),
        showBackground: true,
        // 背景色
        // backgroundStyle: {
        //   color: 'rgba(58,138,143,0.2)',
        // },
        // barBorderRadius: [0, 30, 30, 0],
        backgroundStyle: {
          color: '#194B72',
        },
        // 每个柱的样式
        // itemStyle: {
        //   color: '#479ad3', // 柱颜色
        //   barBorderRadius: 5, // 圆角
        //   shadowColor: 'rgba(0,0,0,0.3)', // 阴影
        //   shadowBluer: 5, // 阴影模糊
        // },
        // 柱宽
        barWidth: 12,
        // 轴上文字
        label: {
          show: true,
          position: 'right',
          textStyle: {
            color: '#fff',
          },
        },
      },
    ],
  }
  myChart.setOption(options)
}

watch(() => props.data, renderChart)
</script>

<style scoped></style>
