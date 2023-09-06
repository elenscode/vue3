<script setup lang="ts">
// import { ref } from 'vue';
import PieChart from './components/PieChart.vue';
import ScatterChart from './components/ScatterChart.vue';
import LineChart from './components/LineChart.vue';
import GridTable from './components/GridTable.vue';
// import { useListStore } from './stores/list';

// const text = ref("");
// const list = useListStore();

</script>

<template>
  <v-app>
    <v-main>
      <v-container fluid>
        <v-row justify="center">
          <v-col cols="10">
            <div class="d-flex justify-center">
              <PieChart title="lots" />
              <PieChart title="partid" />
              <PieChart title="cmt" />
              <PieChart title="pie" />
            </div>
          </v-col>
        </v-row>
        <v-row justify="center">
          <v-col cols="10">
            <LineChart />
          </v-col>
        </v-row>
        <v-row justify="center">
          <v-col cols="10">
            <ScatterChart />

          </v-col>
        </v-row>
        <v-row justify="center">
          <v-col cols="10">
            <GridTable />
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}

.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}

.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
