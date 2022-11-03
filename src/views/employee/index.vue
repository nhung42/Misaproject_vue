<template>
    <div class="employee-page page-view">
      <v-employee> </v-employee>
    </div>
  </template>
  
  <script>
  import { defineAsyncComponent } from "vue";
  
  export default {
    components: {
      VEmployee: defineAsyncComponent(() => import("./Employee.vue")),
    },
    setup() {},
  };
  </script>