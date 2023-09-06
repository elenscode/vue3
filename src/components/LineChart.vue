<script setup lang="ts">
import { use } from "echarts/core";
import { LineChart } from "echarts/charts";
import {
  GridComponent,
  TitleComponent,
  LegendComponent
} from "echarts/components";
import { shallowRef } from "vue";
import VChart from 'vue-echarts';
import getData from "../data/line.js";

use([LineChart, GridComponent, TitleComponent, LegendComponent]);

const option = shallowRef(getData());
</script>

<template>
  <v-chart class="chart" :option="option" autoresize />
</template>

<style scoped>
.chart {
  height: 40vh;
}
</style>
