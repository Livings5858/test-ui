<script setup>
import ShowData from '../components/ShowData.vue'
import TemperatureChart from '../components/TemperatureChart.vue';
import ImagePlaceholder from '../components/ImagePlaceholder.vue';
</script>

<template>
  <div class="styled-div">
    <ShowData />
  </div>
  <div class="container">
    <ImagePlaceholder/>
    <TemperatureChart />
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

</style>
