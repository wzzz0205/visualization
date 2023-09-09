<!-- 雷达图 -->
<template>
  <div>
    <div class="text-lg">【云端报警风险】</div>
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
  '#00FF7F',
  '#9933FA',
  '#00C78C',
]
const renderChart = () => {
  const options = {
    // 雷达图的坐标系配置
    radar: {
      //指示器名称配置
      name: {
        textStyle: {
          color: '#05d5ff',
          fontSize: 14,
        },
      },
      //雷达图绘制类型，支持 'polygon' 和 'circle'
      shape: 'polygon',
      //中心（圆心）坐标，数组的第一项是横坐标，第二项是纵坐标。设置成百分比时第一项是相对于容器宽度，第二项是相对于容器高度。
      center: ['50%', '50%'],
      //半径  占容器宽高比列
      radius: '80%',
      //坐标系起始角度
      startAngle: 120,
      //坐标轴线
      axisLine: {
        lineStyle: {
          color: 'rgba(211, 253, 250, 0.8)',
        },
      },
      //坐标轴在 grid 区域中的分隔线
      splitLine: {
        show: true,
        lineStyle: {
          width: 2,
          color: 'rgba(5,213,255,.8)',
        },
      },
      //指示器
      indicator: props.data.risks.map((item, index) => ({
        name: item.name,
        max: 100,
        color: colorList[index],
      })),
      //分隔区域
      splitArea: {
        show: false,
      },
    },
    // 极坐标系配置
    polar: {
      //中心 圆心
      center: ['50%', '50%'],
      //极坐标系的半径
      radius: '0%',
    },
    // 角度轴
    angleAxis: {
      min: 0,
      interval: 5,
      clockwise: false,
    },
    // 半径轴
    radiusAxis: {
      min: 0,
      //强制设置坐标轴分割间隔。
      interval: 20,
      //坐标轴在 grid 区域中的分隔线
      splitLine: {
        show: false,
      },
    },
    tooltip: {
      //提示框
      trigger: 'item', //触发类型
    },
    // 核心配置
    series: {
      type: 'radar',
      //标记的图形
      symbol: 'circle',
      symbolSize: 10,
      itemStyle: {
        normal: {
          color: '#05d5ff',
          opacity: 0.8,
        },
      },
      //区域填充样式
      areaStyle: {
        normal: {
          color: '#05d5ff',
          opacity: 0.5,
        },
      },
      lineStyle: {
        width: 2,
        color: '#05d5ff',
      },
      label: {
        normal: {
          show: true,
          color: '#fff',
        },
      },
      data: [props.data.risks.map((item) => item.value)],
    },
  }
  myChart.setOption(options)
}

watch(() => props.data, renderChart)
</script>

<style scoped></style>
