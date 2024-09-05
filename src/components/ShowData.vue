<template>
  <div>
    <div class="container">
      <h1>MQTT 数据展示</h1>
    </div>
    <ul class="no-bullets">
      <!-- <li v-for="item in items" :key="item">{{ item }}</li> -->
      <li
        v-for="(item, index) in props.items"
        :key="index"
        :style="{
          color: index === props.items.length - 1 && item.includes('Fire') ? 'red' : 'white'
        }"
      >
        {{ item }}
      </li>
    </ul>
  </div>
</template>

<script setup>
const props = defineProps(['items'])
</script>

<style scoped>

.no-bullets {
  list-style-type: none; /* 去掉前面的点 */
  padding-left: 0; /* 可选：去掉左侧默认内边距 */
}

.container {
  margin-bottom: 10px;
}
</style>