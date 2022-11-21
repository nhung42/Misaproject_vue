<template>
  <li class="item-dropdown" tabindex="-1" @click="emitClick" @keyup.enter="emitClick" @keyup.space="emitClick">
    <label class="text-item">{{ dataItem[displayField] }}</label>
  </li>
</template>
<script>
export default {
  name: "MsItem",
  components: {
  },
  props: {
    icon: {
      type: [Array, String],
      default: null,
    },
    disabled: {
      default: false,
      type: Boolean,
    },
    displayField: {
      default: null,
      type: String,
    },

    dataItem: {
      default: {},
    },
  },
  methods: {
    /**
     * Click vào item ropdown
     * @param {*} e 
     * @author DuongNhung
     */
    emitClick(e) {
      console.log(e);

      this.$emit("menu-item-click", this.dataItem);
    },
  },
  setup() {

    return {};
  },
};
</script>
<style lang="scss" scoped>
@import url("https://fonts.googleapis.com/css2?family=Roboto:wght@400;500;700&display=swap");

.item-dropdown {
  width: 100%;
  height: 28px !important;
  min-height: 28px;
  overflow: hidden;
  padding: 4px 0;
  outline: none;
  display: flex;
  align-items: center;
  font-family: Roboto;
  font-size: 14px;


  &:active,
  &:focus,
  &:focus-within {
    transition: 0.5s;

    cursor: pointer;

  }

  &:hover {
    transition: 0.2s;
    background-color: #a2f399;
    cursor: pointer;
    border-radius: 4px;

  }


  .icon_left-dropdown {
    width: 36px;
    height: 36px;
    min-width: 36px;
    background-image: none;
  }

  .text-item {
    font-size: 14px;
    text-overflow: ellipsis;
    white-space: nowrap;
    overflow: hidden;
    margin-left: 16px;
  }
}

.selected {
  transition: 0.2s;
  background-color: #a2f399;

  .inline-block {
    .item-dropdown {}
  }
}
</style>
  