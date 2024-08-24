<template>
  <div class="image-container">
    <img v-if="imageSrc" :src="imageSrc" alt="Image" />
    <div v-else class="placeholder"></div>
  </div>
</template>

<script setup>
import axios from 'axios';
import { onMounted, ref } from 'vue'

const imageSrc = ref("");

const fetchData = () => {
      axios.get('http://localhost:8001/api/imagepath')
        .then(response => {
          imageSrc.value = response.data;
        })
        .catch(error => {
          console.error("There was an error fetching the data!", error);
        });
};

onMounted(() => {
  fetchData();
  setInterval(fetchData, 5000);
});
</script>

<style scoped>
.image-container {
  width: 600px;  /* 可以根据需要调整宽度 */
  height: 400px; /* 可以根据需要调整高度 */
  background-color: gray; /* 当没有图片时的背景颜色 */
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}

.shadow-image {
  width: 100%; /* 设置图片宽度 */
  max-width: 500px; /* 设置最大宽度 */
  height: auto; /* 保持图片比例 */
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2); /* 添加阴影 */
  border-radius: 10px; /* 可选：圆角效果 */
}

img {
  width: 100%;
  height: 100%;
  object-fit: cover; /* 确保图片覆盖整个容器 */
}

.placeholder {
  width: 100%;
  height: 100%;
  background-color: gray; /* 占位符背景颜色 */
}
</style>
