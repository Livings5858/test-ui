<template>
  <div>
    <h1>Data from Spring Boot</h1>
    <ul>
      <li v-for="item in items" :key="item">{{ item }}</li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      items: []
    };
  },
  methods: {
    fetchData() {
      axios.get('http://192.168.3.8:8001/api/data')
        .then(response => {
          this.items = response.data;
        })
        .catch(error => {
          console.error("There was an error fetching the data!", error);
        });
    }
  },
  created() {
    this.fetchData();
    // 每5秒钟请求一次数据
    setInterval(this.fetchData, 5000);
  }
};
</script>
