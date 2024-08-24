<script setup>
import ShowData from '../components/ShowData.vue'
import TemperatureChart from '../components/TemperatureChart.vue';
import ImagePlaceholder from '../components/ImagePlaceholder.vue';

import axios from 'axios';
import {ref, onMounted} from 'vue'

const temperatures = ref([])
const items = ref([]);
const imageSrc = ref("");
const fireInfo = ref("");

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
};

const fetchData = () => {
      axios.get('http://localhost:8001/api/data')
        .then(response => {
          items.value = response.data;
        })
        .catch(error => {
          console.error("There was an error fetching the data!", error);
        });
};

const fetchImagePath = () => {
      axios.get('http://localhost:8001/api/imagepath')
        .then(response => {
          imageSrc.value = response.data;
        })
        .catch(error => {
          console.error("There was an error fetching the data!", error);
        });
};

const fetchFireInfo = () => {
      axios.get('http://localhost:8001/api/fireinfo')
        .then(response => {
          fireInfo.value = response.data;
        })
        .catch(error => {
          console.error("There was an error fetching the data!", error);
        });
};

const clearTemperature = () => {
  temperatures.value = [];
  localStorage.setItem('temperatures', JSON.stringify(temperatures.value));
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

const loadTemperatures = () => {
  // 从 localStorage 中恢复数据
  const storedTemperatures = localStorage.getItem('temperatures');
  if (storedTemperatures) {
    temperatures.value = JSON.parse(storedTemperatures);
  }
};

onMounted(() => {
  loadTemperatures();
  fetchTemperature();
  fetchData();
  fetchImagePath();
  fetchFireInfo();
  setInterval(fetchTemperature, 3000);
  setInterval(fetchData, 3000);
  setInterval(fetchImagePath, 3000);
  setInterval(fetchFireInfo, 3000);
});
</script>

<template>
  <div class="styled-div">
    <ShowData :items="items"/>
  </div>
  <div class="container">
    <ImagePlaceholder :imageSrc="imageSrc"/>
    <div>
      <TemperatureChart />
      <div class="container2">
        <button class="custom-button" @click="clearTemperature">清除历史温度数据</button>
        <button class="custom-button" @click="sendController">发送控制命令</button>
      </div>
    </div>
  </div>
</template>


<style scoped>
.container {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

/* 调整中间间隔 */
.container {
  gap: 80px; /* 这里是你想要的间隔像素数 */
}

.styled-div {
  display: flex;
  justify-content: center;
  align-items: top;
  height: 200px; /* 设置div的高度，保证垂直居中 */
  /* background-color: #f0f8ff; 背景颜色 */
  color: #9d9d9d; /* 字体颜色 */
  font-family: 'Arial', sans-serif; /* 字体样式 */
  font-size: 20px; /* 字体大小 */
  font-weight: bold; /* 字体粗细 */
  text-align: center; /* 文本居中 */
  border-radius: 15px; /* 圆角 */
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); /* 阴影效果 */
  padding: 10px; /* 内边距 */
  margin: 2px; /* 外边距 */
  margin-bottom: 20px;
}


.container2 {
  display: flex;
  justify-content: space-around;
}

/* 调整中间间隔 */
.container2 {
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
