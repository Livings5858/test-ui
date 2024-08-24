<template>
  <div>
    <div ref="chart" style="width: 600px; height: 400px;"></div>
    <div class="container">
    <button class="custom-button" @click="clearTemperature">清除历史温度数据</button>
    <button class="custom-button" @click="sendController">发送控制命令</button>
    </div>
  </div>
</template>

<script setup>
import * as echarts from 'echarts';
import axios from 'axios';
import { onMounted, ref } from 'vue'

const temperatures = ref([])
// 声明一个 ref 来存放该元素的引用
// 必须和模板里的 ref 同名
const chart = ref(null)

// 初始化图表
const initChart = () => {
  chart.value = echarts.init(chart.value);
  loadTemperatures();
  updateChart();
};

const fetchTemperature = () => {
  axios.get('http://localhost:8001/api/temperature')
    .then(response => {
      temperatures.value.push(response.data);
    })
    .catch(error => {
      console.error("There was an error fetching the data!", error);
    });

  // 持久化存储数据到 localStorage
  localStorage.setItem('temperatures', JSON.stringify(temperatures.value));
  // 更新图表
  updateChart();
};

const clearTemperature = () => {
  temperatures.value = [];
  localStorage.setItem('temperatures', JSON.stringify(temperatures.value));
  updateChart();
}

const sendController = ()=>{
  axios.get('http://localhost:8001/api/control?content="test cmd to sensor"')
    .then(response => {
      // temperatures.value.push(response.data);
    })
    .catch(error => {
      console.error("There was an error fetching the data!", error);
    });
}

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
  fetchTemperature();
  setInterval(fetchTemperature, 5000);
});
</script>

<style scoped>
/* 样式可根据需要调整 */
.container {
  display: flex;
  justify-content: space-around;
}

/* 调整中间间隔 */
.container {
  gap: 10px; /* 这里是你想要的间隔像素数 */
}

.custom-button {
  width: 150px;           /* 固定宽度 */
  height: 50px;           /* 固定高度 */
  background-color: #4CAF50; /* 按钮背景色 */
  color: white;           /* 按钮文字颜色 */
  border: none;           /* 去掉边框 */
  border-radius: 8px;     /* 圆角边框 */
  font-size: 16px;        /* 文字大小 */
  font-weight: bold;      /* 文字加粗 */
  cursor: pointer;        /* 鼠标悬停时显示手型 */
  text-align: center;     /* 文字居中 */
  line-height: 50px;      /* 让文字在按钮中垂直居中 */
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1); /* 添加阴影效果 */
  transition: background-color 0.3s ease; /* 背景色过渡效果 */
}

.custom-button:hover {
  background-color: #45a049; /* 鼠标悬停时改变背景色 */
}

.custom-button:active {
  background-color: #3e8e41; /* 按下时改变背景色 */
}

.custom-button:disabled {
  background-color: #cccccc; /* 禁用状态时的背景色 */
  cursor: not-allowed;       /* 禁用时显示禁止图标 */
}

</style>
