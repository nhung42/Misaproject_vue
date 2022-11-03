<template>
  <div id="app">

    <the-left-menu></the-left-menu>
    <the-header></the-header>
    <div class="app-content">
      <div class="app-content-router-view">
        <the-employee></the-employee>
      </div>
    </div>
  </div>
</template>

<script>
import TheHeader from '@/layout/header/TheHeader.vue';
import TheLeftMenu from '@/layout/sidebar/TheSideBar.vue';
import TheEmployee from "@/views/employee/Employee.vue"

export default {
  components: {
    TheHeader,
    TheLeftMenu,
    TheEmployee
  }
}
</script>

<style lang="scss">
//reset style
* {
  padding: 0;
  margin: 0;
}

#app {
  width: 100%;
  height: 100vh;
}

#app {
  .app-content {
    width: calc(100% - 200px);
    height: 100vh;
    display: flex;

    &-router-view {
      flex-grow: 2;
      padding: 24px;
      background-color: bisque;
    }
  }
}
</style>
