<template>
  <div id="app">
    <the-left-menu></the-left-menu>
    <the-header></the-header>
    <div class="app-content">
      <div class="app-content-router-view">
        <Suspense>
          <ms-employee></ms-employee>
        </Suspense>
      </div>
    </div>
  </div>
</template>

<script>
import TheHeader from '@/layout/header/TheHeader.vue';
import TheLeftMenu from '@/layout/sidebar/TheSideBar.vue';
import MsEmployee from "@/views/employee/Employee.vue"

export default {
  components: {
    TheHeader,
    TheLeftMenu,
    MsEmployee
  }
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;500;700&display=swap');

//reset style
* {
  padding: 0;
  margin: 0;
}

#app {
  width: 100%;
  height: 100vh;
  box-sizing: border-box;
}

#app {
  .app-content {
    width: calc(100% - 200px);
    height: calc(100vh - 56px);
    display: flex;
    font-family: Roboto;

    &-router-view {
      width: 100%;
      background-color: #eceef1;
    }
  }
}
</style>
