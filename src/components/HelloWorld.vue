<template>
  <div class="app">
    <ag-grid-vue
      class="ag-theme-alpine"
      style="width: 100%; height: 500px;"
      :rowData="rowData"
      :columnDefs="columnDefs"
      :rowSelection="'multiple'"
      :defaultColDef="defaultColDef"
      @rowSelected="onRowSelected"
    />
  </div>
</template>

<script>
import { AgGridVue } from "ag-grid-vue3";

export default {
  name: "App",
  components: {
    AgGridVue,
  },
  data() {
    return {
      // Row data for the grid
      rowData: [
        { id: 1, name: "John Doe", age: 25, disable: true },
        { id: 2, name: "Jane Smith", age: 30, disable: false },
        { id: 3, name: "Alice Johnson", age: 35, disable: false },
      ],
      // Column definitions
      columnDefs: [
        {
          headerCheckboxSelection: true,
          headerCheckboxSelectionFilteredOnly: true,
          width: 50,
          lockPosition: true,
          pinned: "left",
          cellRenderer: (params) => {
            const checkbox = document.createElement("input");
            checkbox.type = "checkbox";
            checkbox.disabled = params.data.disable; // Disable checkbox if row is disabled
            checkbox.checked = params.node.isSelected();

            if (!params.data.disable) {
              checkbox.addEventListener("change", (event) => {
                params.node.setSelected(event.target.checked);
              });
            }

            return checkbox;
          },
        },
        { field: "id", headerName: "ID" },
        { field: "name", headerName: "Name" },
        { field: "age", headerName: "Age" },
        {
          field: "action",
          headerName: "Action",
          cellRenderer: (params) => {
            const button = document.createElement("button");
            button.textContent = "Disable";
            button.className = "action-btn";

            button.addEventListener("click", () => {
              params.data.disable = true; // Set the disable flag to true
              params.api.refreshCells({ force: true }); // Refresh the grid to update the UI
            });

            return button;
          },
        },
      ],
      defaultColDef: {
        sortable: true,
        filter: true,
        resizable: true,
      },
    };
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

/* Style for the action button */
.action-btn {
  padding: 5px 10px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.action-btn:hover {
  background-color: #0056b3;
}
</style>
