<template>
  <div>
    <h1>Data from Spring Boot</h1>
    <ul class="no-bullets">
      <li v-for="item in items" :key="item">{{ item }}</li>
    </ul>
  </div>
</template>

<script setup>
import axios from 'axios';
import {ref, onMounted} from 'vue'
const items = ref([]);

const fetchData = () => {
      axios.get('http://192.168.3.8:8001/api/data')
        .then(response => {
          items.value = response.data;
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

.no-bullets {
  list-style-type: none; /* 去掉前面的点 */
  padding-left: 0; /* 可选：去掉左侧默认内边距 */
}
</style>