<template>
  <div class="app">
    <ag-grid-vue
      class="ag-theme-alpine"
      style="width: 100%; height: 500px;"
      :rowData="rowData"
      :columnDefs="columnDefs"
      :rowSelection="'multiple'"
      :defaultColDef="defaultColDef"
      :frameworkComponents="frameworkComponents"
      :isRowSelectable="isRowSelectable"
      @rowSelected="onRowSelected"
    />
  </div>
</template>

<script>
import { AgGridVue } from "ag-grid-vue3";
import ActionButtonRenderer from "./ActionButtonRenderer.vue";
import SelectionControlRenderer from "./SelectionControlRenderer.vue";

export default {
  name: "App",
  components: {
    AgGridVue,
  },
  data() {
    return {
      // Row data for the grid
      rowData: [
        { id: 1, name: "John Doe", age: 25, year: 2002 },
        { id: 2, name: "Jane Smith", age: 30, year: 2005 },
        { id: 3, name: "Alice Johnson", age: 35, year: 2004 },
      ],
      // Column definitions
      columnDefs: [
        {
          headerCheckboxSelection: true,
          headerCheckboxSelectionFilteredOnly: true,
          width: 50,
          lockPosition: true,
          pinned: "left",
          cellRendererFramework: "SelectionControlRenderer", // Custom renderer for checkboxes
        },
        { field: "id", headerName: "ID" },
        { field: "name", headerName: "Name" },
        { field: "age", headerName: "Age" },
        { field: "year", headerName: "Year" },
        {
          field: "action",
          headerName: "Action",
          cellRendererFramework: "ActionButtonRenderer", // Use Vue component for rendering action button
        },
      ],
      frameworkComponents: {
        ActionButtonRenderer,
        SelectionControlRenderer,
      },
      defaultColDef: {
        sortable: true,
        filter: true,
        resizable: true,
      },
    };
  },
  computed: {
    isRowSelectable() {
      return (node) => (node.data ? node.data.year <= 2004 : false); // Rows with year <= 2004 are selectable
    },
  },
  methods: {
    onRowSelected(event) {
      console.log("Row Selected:", event.node.data);
    },
  },
};
</script>

<style>
/* Add AG Grid theme */
@import "ag-grid-community/styles/ag-grid.css";
@import "ag-grid-community/styles/ag-theme-alpine.css";

.app {
  margin: 20px;
}
</style>
