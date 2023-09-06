<template>
  <div style="height: 30vh;" @keydown.prevent.ctrl.v="paste">
    <ag-grid-vue class="ag-theme-alpine" style="width: 100%; height: 100%;" :columnDefs="columnDefs" :rowData="rowData"
      :defaultColDef="defaultColDef" rowSelection="multiple" animateRows="true" @grid-ready="onGridReady"
      :suppressPaste="true">
    </ag-grid-vue>
  </div>
</template>

<script setup lang="ts">
import { AgGridVue } from "ag-grid-vue3";  // the AG Grid Vue Component
import { onMounted, ref } from "vue";

import "ag-grid-community/styles/ag-grid.css"; // Core grid CSS, always needed
import "ag-grid-community/styles/ag-theme-alpine.css"; // Optional theme CSS

const gridApi = ref(null); // Optional - for accessing Grid's API

// Obtain API from grid's onGridReady event
const onGridReady = (params: { api: null; }) => {
  gridApi.value = params.api;
};
const rowData = ref([{ desc: '', expression: '', bins: '' }, { desc: '', expression: '', bins: '' }]); // Set rowData to Array of Objects, one Object per Row

const extractNumbers = (str: string) => {
  let results = [];
  let parts = str.split(',');

  for (let part of parts) {
    if (part.includes('~')) {
      let [start, end] = part.split('~').map(s => parseInt(s.replace(/[^\d]/g, ''), 10));
      for (let i = start; i <= end; i++) {
        results.push(i);
      }
    } else {
      let num = parseInt(part.replace(/[^\d]/g, ''), 10);
      results.push(num);
    }
  }

  return results.join(',');
}


async function paste() {

  try {
    const clipText = await navigator.clipboard.readText();

    const data = [...clipText.split('\n')];
    const returnObj = [];
    const emptyLastRow =
      data[data.length - 1][0] === '' && data[data.length - 1].length === 1;
    if (emptyLastRow) {
      data.splice(data.length - 1, 1);
    }
    for (let part of data) {
      const tokens = part.split('\t');
      if (tokens.length !== 2) continue;
      const [desc, expression] = tokens;
      returnObj.push({ desc, expression, bins: extractNumbers(expression) });
    }

    // 클립보드 read에 성공했을     
    const params = {
      // force: isForceRefreshSelected(),
      // suppressFlash: isSuppressFlashSelected(),
    };
    if (returnObj.length > 0)
      rowData.value = [...returnObj];
    gridApi.value.refreshCells(params);
  } catch {

    // 클립보드 read에 실패했을 때
    console.log('fail');
  }

}


// Each Column Definition results in one Column.
const columnDefs = ref([
  { field: "desc" },
  { field: "expression" },
  { field: "bins" },
]);

// DefaultColDef sets props common to all Columns
const defaultColDef = {
  sortable: true,
  filter: true,
  editable: true,
  flex: 1
};

// Example load data from server
onMounted(() => {

});

</script>

<style lang="scss"></style>