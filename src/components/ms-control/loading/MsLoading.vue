<template>
  <div class="loading">
    <div class="half-circle-spinner">
      <div class="circle circle-1"></div>
      <div class="circle circle-2"></div>
    </div>
  </div>
</template>
<script>
export default {
  name: "MsLoading",
  props: {
    isLoading: {
      default: false,
    },
  },
  setup() {
    //const { proxy } = getCurrentInstance();
  },
};
</script>
<style lang="scss" scoped>
@import "@/style/components/MsLoading.scss";
</style>
