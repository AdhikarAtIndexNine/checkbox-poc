<template>
    <div>
      <input
        type="checkbox"
        :checked="params.node.isSelected()"
        :disabled="!params.api.isRowSelectable(params.node)"
        @change="toggleSelection"
      />
    </div>
  </template>
  
  <script>
  export default {
    props: ["params"],
    methods: {
      toggleSelection(event) {
        this.params.node.setSelected(event.target.checked);
      },
    },
  };
  </script>
  
  <style scoped>
  /* Optional styling */
  </style>
  