<!-- 竖向柱状图 -->
<template>
  <div>
    <div class="text-lg">【竖向柱状图】</div>
    <div ref="target" class="w-full h-full"></div>
  </div>
</template>

<script setup>
import { onMounted, ref, watch } from 'vue'
import * as echarts from 'echarts'

const props = defineProps({
  data: {
    typeof: Object,
    require: true,
  },
})
let myChart = null
const target = ref(null)

onMounted(() => {
  myChart = echarts.init(target.value)
  renderChart()
})

let colorList = [
  'rgba(255, 215, 0, 1)',
  'rgba(255, 153, 18, 1)',
  'rgba(250, 235, 215, 1)',
  '#7FFFD4',
  '#A020F0',
  '#00FF00',
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
      type: 'category',
      data: props.data.servers.map((item) => item.name),
      axisLabel: {
        color: '#9eb1c8',
        fontSize: 16,
        fontWeight: 'bold',
      },
    },
    yAxis: {
      show: false,
      type: 'value',
      max: function (value) {
        return parseInt(value.max * 1.2)
      },
    },
    grid: {
      top: 10,
      left: 0,
      right: 0,
      bottom: 45,
    },
    tooltip: {
      //提示框
      trigger: 'item', //触发类型
    },
    series: [
      {
        type: 'bar',
        data: props.data.servers.map((item, index) => {
          return {
            name: item.name,
            value: item.value,
            itemStyle: {
              borderRadius: [30, 30, 0, 0],
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
          }
        }),
        showBackground: true,
        // 背景色
        // backgroundStyle: {
        //   color: 'rgba(58,138,143,0.2)',
        // },
        // barBorderRadius: [30, 0, 0, 30],
        backgroundStyle: {
          color: '#194B72',
        },
        // 柱宽
        barWidth: 12,
        // 轴上文字
        label: {
          show: true,
          position: 'top',
          textStyle: {
            color: '#fff',
          },
          formatter: '{c}%',
        },
      },
    ],
  }
  myChart.setOption(options)
}

watch(() => props.data, renderChart)
</script>

<style scoped></style>
