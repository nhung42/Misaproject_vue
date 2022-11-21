
<template>
  <div class="m-toast">
    <div class="m-toast-main">
      <div class="left-toast">
        <div class="icon-succes"></div>
        <div class="toast-content">
          <b for="" class="title-toast">Thành công!</b>
          <label for="">Bạn đã {{ toastAct }} thành công nhân viên</label>
        </div>
      </div>
      <div class="icon-close" @click="closeOpenToast()"></div>
    </div>
  </div>
</template>
<script>
export default {
  name: "MsToastMessage",
  props: ["toastAct"],
  methods: {
    /**
     * Thực hiện đóng Toast
     * Author: DuongNhung
     */
    closeOpenToast() {
      this.$emit('closeOpenToast')
    },
  },
};
</script>
<style scoped>
.m-toast {
  position: fixed;
  right: 32px;
  bottom: 32px;
  z-index: 9999;
  display: block;
}

.m-toast-main {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 400px;
  height: 80px;
  background-color: #eef8ec;
  border-left: 5px solid #50b83c;
  border-radius: 4px;
  /* animation: toastLeft ease 0.3s, toastOut linear 1s 3s forwards; */
}

.left-toast {
  display: flex;
  padding-left: 10px;
}

.toast-content {
  align-items: center;
  flex-direction: column;
  justify-content: start;
}

.title-toast {
  display: block;
  font-size: 20px;
}

.ml-16 {
  margin-left: 16px;
}

.mr-16 {
  margin-right: 16px;
}

.mr-8 {
  margin-right: 8px;
}

@keyframes toastLeft {
  from {
    opacity: 0;
    transform: translateX(calc(100% + 32px));
  }

  to {
    opacity: 1;
    transform: translateX(0);
  }
}

@keyframes toastOut {
  to {
    opacity: 0;
  }
}
</style>
