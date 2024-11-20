<template>
  <div style="height: 100%">
    <div class="example-wrapper">
      <div class="example-header">
        <label>
          <span>Hide disabled checkboxes:</span>
          <input
            id="toggle-hide-checkbox"
            type="checkbox"
            checked
            @change="toggleHideCheckbox"
          />
        </label>
      </div>
      <ag-grid-vue
        style="width: 100%; height: 100vh;"
        :class="themeClass"
        :columnDefs="columnDefs"
        :defaultColDef="defaultColDef"
        :rowSelection="rowSelection"
        :rowData="rowData"
        @grid-ready="onGridReady"
      />
    </div>
  </div>
</template>

<script>
import { ref, shallowRef, onBeforeMount } from "vue";
import { AgGridVue } from "ag-grid-vue3";
import "ag-grid-charts-enterprise";
import "ag-grid-community/styles/ag-grid.css";
import "ag-grid-community/styles/ag-theme-quartz.css";

export default {
  name: "VueExample",
  components: {
    AgGridVue,
  },
  setup() {
    const columnDefs = ref([
      { field: "athlete" },
      { field: "sport" },
      { field: "year", maxWidth: 120 },
    ]);
    const gridApi = shallowRef();
    const defaultColDef = ref({
      flex: 1,
      minWidth: 100,
    });
    const rowSelection = ref(null);
    const rowData = ref(null);
    const themeClass = "ag-theme-quartz";

    // Setup logic for row selection with hideDisabledCheckboxes
    onBeforeMount(() => {
      rowSelection.value = {
        mode: "multiRow",
        hideDisabledCheckboxes: true,
        isRowSelectable: (node) =>
          node.data ? node.data.year <= 2004 : false,
      };
    });

    // Function to toggle hiding disabled checkboxes
    const toggleHideCheckbox = () => {
      const checkboxValue = document.querySelector("#toggle-hide-checkbox")
        ?.checked ?? false;
      gridApi.value.setGridOption("rowSelection", {
        mode: "multiRow",
        isRowSelectable: (node) =>
          node.data ? node.data.year <= 2004 : false,
        hideDisabledCheckboxes: checkboxValue,
      });
    };

    // Grid ready event handler
    const onGridReady = (params) => {
      gridApi.value = params.api;

      const updateData = (data) => {
        rowData.value = data;
      };

      fetch("https://www.ag-grid.com/example-assets/small-olympic-winners.json")
        .then((resp) => resp.json())
        .then((data) => updateData(data));
    };

    return {
      columnDefs,
      gridApi,
      defaultColDef,
      rowSelection,
      rowData,
      themeClass,
      toggleHideCheckbox,
      onGridReady,
    };
  },
};
</script>

<style>
.example-wrapper {
  margin: 10px;
}
.example-header {
  margin-bottom: 10px;
}
</style>
