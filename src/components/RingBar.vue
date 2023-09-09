<!-- 环形图 -->
<template>
  <div>
    <div class="text-lg">【大区异常处理】</div>
    <div ref="target" class="w-full h-full"></div>
  </div>
</template>

<script setup>
import { ref, onMounted, watch } from 'vue'
import * as echarts from 'echarts'

const props = defineProps({
  data: {
    typeof: Object,
    required: true,
  },
})

// 1. 初始化 echarts 实例
let mChart = null
const target = ref(null)

onMounted(() => {
  mChart = echarts.init(target.value)
  renderChart()
})

let colorList = ['#292421', '#C0C0C0', '#808A87', '#708069f', '#DCDCDC']
const randomRGB = () => {
  const R = Math.floor(Math.random() * 255)
  const G = Math.floor(Math.random() * 255)
  const B = Math.floor(Math.random() * 255)
  return `rgb(${R},${G},${B})`
}

const randomRGB2 = () => {
  //生成0-5随机数
  return colorList[Math.floor(Math.random() * 4)]
}

const getSeriesData = () => {
  const series = []
  props.data.abnormals.forEach((item, index) => {
    // 上层
    series.push({
      name: item.name,
      type: 'pie',
      clockWise: false, //饼图的扇区是否是顺时针排布
      hoverAnimation: false,
      radius: [78 - index * 15 + '%', 68 - index * 15 + '%'],
      center: ['55%', '55%'],
      label: {
        show: false,
      },
      data: [
        {
          value: item.value,
          name: item.name,
          itemStyle: {
            borderColor: '#fff',
            borderWidth: 2,
            color: randomRGB(),
          },
        },
        {
          value: 1000,
          itemStyle: {
            color: 'rgba(0,0,0,0)',
            borderWidth: 0,
          },
          tooltip: {
            show: false,
          },
          hoverAnimation: false,
        },
      ],
    })
    // 底层
    series.push({
      name: item.name,
      type: 'pie',
      silent: true,
      z: 1,
      clockWise: false,
      hoverAnimation: false,
      radius: [80 - index * 15 + '%', 68 - index * 15 + '%'],
      center: ['55%', '55%'],
      label: {
        show: false,
      },
      data: [
        {
          value: 7.5,
          itemStyle: {
            color: randomRGB2(), //rgba(3,31,62)
            borderColor: '#000',
            borderWidth: 2,
          },
          tooltip: {
            show: false,
          },
          hoverAnimation: false,
        },
        {
          value: 2.5,
          itemStyle: {
            color: 'rgba(0,0,0,0)',
            borderWidth: 0,
          },
          tooltip: {
            show: false,
          },
          hoverAnimation: false,
        },
      ],
    })
  })
  return series
}

// 2. 构建 option 配置对象
const renderChart = () => {
  const options = {
    // 图例配置
    legend: {
      show: true,
      icon: 'circle',
      top: '8%',
      left: '62%',
      data: props.data.abnormals.map((item) => item.name),
      width: -5,
      itemWidth: 12,
      itemHeight: 12,
      itemGap: 6,
      textStyle: {
        fontSize: 14,
        lineHeight: 5,
        color: '#ffffff',
      },
    },
    // 提示层
    tooltip: {
      show: true,
      trigger: 'item',
      formatter: '{b}:{c}({d}%)', //{a}<br>
    },
    // Y轴
    yAxis: [
      {
        type: 'category',
        inverse: true,
        axisLine: {
          show: false,
        },
      },
    ],
    // X轴
    xAxis: [
      {
        show: false,
      },
    ],
    // 核心配置
    series: getSeriesData(),
  }
  // 3. 通过 实例.setOptions(option)
  mChart.setOption(options)
}

watch(() => props.data, [renderChart, randomRGB2])
</script>

<style scoped></style>
