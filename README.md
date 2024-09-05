# MQTT 数据展示前端实现

## 介绍

基于Vue.js框架，集成了Echarts和Axios，实现获取后端数据并展示。

* Vue.js： https://cn.vuejs.org/
* Echarts：https://echarts.apache.org/zh/index.html
* Axios: https://axios.nodejs.cn/docs/intro

## 效果示例

<img src="public\image.png"  width="600" />


## 目录结构

```
src/
...
├── components
│   ├── ImagePlaceholder.vue   // 图片展示组件
│   ├── ShowData.vue           // 数据列表展示组件
│   ├── TemperatureChart.vue   // 温度折线图组件
...
└── views
    └── HomeView.vue           // 父组件，负责统一向后端请求数据，并分发给子组件prop。
```

## 快速开始

### 1. 安装依赖

```sh
npm install
```

### 2. 运行（开发环境）

```sh
npm run dev
```

### 3. 编译构建（生产环境）

```sh
npm run build
```

## 关联仓库

* MQTT 数据展示后端实现：https://github.com/Livings5858/test-server
