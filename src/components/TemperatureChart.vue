<template>
  <div>
    <button @click="fetchTemperature">获取温度数据</button>
    <div ref="chart" style="width: 600px; height: 400px;"></div>
  </div>
</template>

<script setup>
import * as echarts from 'echarts';
import { onMounted, ref } from 'vue'

const temperatures = ref([])
const chart = ref(null)

// 初始化图表
const initChart = () => {
  chart.value = echarts.init(chart.value);
  loadTemperatures();
  updateChart();
};

const fetchTemperature = () => {
  // 模拟异步请求温度数据
  const newTemperature = Math.floor(Math.random() * 35 + 10); // 随机生成10-45度的温度
  temperatures.value.push(newTemperature);

  // 持久化存储数据到 localStorage
  localStorage.setItem('temperatures', JSON.stringify(temperatures.value));

  // 更新图表
  updateChart();
};

const updateChart = () => {
  const option = {
    title: {
      text: '温度变化折线图',
    },
    xAxis: {
      type: 'category',
      data: temperatures.value.map((_, index) => `时间${index + 1}`),
    },
    yAxis: {
      type: 'value',
    },
    series: [
      {
        data: temperatures.value,
        type: 'line',
      },
    ],
  };
  if (chart.value) {
    chart.value.setOption(option);
  }
};

const loadTemperatures = () => {
  // 从 localStorage 中恢复数据
  const storedTemperatures = localStorage.getItem('temperatures');
  if (storedTemperatures) {
    temperatures.value = JSON.parse(storedTemperatures);
  }
};

onMounted(() => {
  initChart();
});
</script>

<style scoped>
/* 样式可根据需要调整 */
</style>
