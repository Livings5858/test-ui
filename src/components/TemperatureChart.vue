<template>
  <div>
    <button @click="fetchTemperature">获取温度数据</button>
    <div ref="chart" style="width: 600px; height: 400px;"></div>
  </div>
</template>

<script>
import * as echarts from 'echarts';

export default {
  data() {
    return {
      temperatures: [], // 存储温度数据
      chart: null, // ECharts 实例
    };
  },
  methods: {
    fetchTemperature() {
      // 模拟异步请求温度数据
      const newTemperature = Math.floor(Math.random() * 35 + 10); // 随机生成10-45度的温度
      this.temperatures.push(newTemperature);

      // 持久化存储数据到 localStorage
      localStorage.setItem('temperatures', JSON.stringify(this.temperatures));

      // 更新图表
      this.updateChart();
    },
    updateChart() {
      const option = {
        title: {
          text: '温度变化折线图',
        },
        xAxis: {
          type: 'category',
          data: this.temperatures.map((_, index) => `时间${index + 1}`),
        },
        yAxis: {
          type: 'value',
        },
        series: [
          {
            data: this.temperatures,
            type: 'line',
          },
        ],
      };
      this.chart.setOption(option);
    },
    loadTemperatures() {
      // 从 localStorage 中恢复数据
      const storedTemperatures = localStorage.getItem('temperatures');
      if (storedTemperatures) {
        this.temperatures = JSON.parse(storedTemperatures);
      }
    },
  },
  mounted() {
    // 初始化图表
    this.chart = echarts.init(this.$refs.chart);

    // 恢复持久化数据
    this.loadTemperatures();

    // 更新图表
    this.updateChart();
  },
};
</script>

<style scoped>
/* 样式可根据需要调整 */
</style>
