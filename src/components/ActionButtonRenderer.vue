<template>
    <button class="action-btn" @click="disableRow">Disable</button>
  </template>
  
  <script>
  export default {
    props: ["params"],
    methods: {
      disableRow() {
        this.params.data.year = 2000; // Update the year to a value <= 2004
        this.params.api.refreshCells({ force: true }); // Refresh the grid to reflect changes
      },
    },
  };
  </script>
  
  <style scoped>
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
  