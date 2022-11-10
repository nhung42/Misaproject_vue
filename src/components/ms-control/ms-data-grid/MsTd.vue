<template>
  <td :style="styles" :class="[cls, styleAlign]">
    <div class="td-inner" ref="td">
      <div v-if="config.type == ColumnType.Checkbox">
        <ms-checkbox v-model="data"></ms-checkbox>
      </div>

      <div v-else-if="config.type == ColumnType.Action">
        <div class="edit-option">
          <div class="text-edit" @click="handleClickUpdate($event, emp)">Sửa</div>
          <button class="icon-combobox-function" @click="handleClickOption">
          </button>
        </div>
        <div v-show="isShowOption" class="dlg-option" v-if="this.isShowOption == true" @click="closeOption">
          <div @click="handleClickDelete($event, item)" class="option option-delete">Xoá</div>
        </div>
      </div>

      <div v-else-if="config.type == ColumnType.Date">
        {{ formatDate(text) }}
      </div>
      <div v-else-if="config.align == ColumnType.AlignCenter">
        {{ text }}
      </div>
      <div v-else> {{ text }} </div>
    </div>
  </td>
</template>
  
<script>
import MsCheckbox from "@/components/ms-control/ms-check-box/MsCheckBox.vue";
// import Enum from "@/dictionary/enum.js";
import {
  computed, getCurrentInstance,
  onMounted,
  ref,
  watch
} from "vue";
const ColumnType = {
  Text: 'Text',
  Date: 'Date',
  Number: 'Number',
  Checkbox: 'Checkbox',
  AlignCenter: 'AlignCenter',
  AlignLeft: 'AlignLeft',
  AlignRight: 'AlignRight',
  Action: 'Action',
}

export default {
  name: "MsTd",
  components: {
    MsCheckbox,
  },
  props: {
    config: {
      type: Object,
    },
    value: {
      type: String,
      default: null,
    },
    emp: {
      type: Object
    }
  },
  methods: {
    formatDate(dob) {
      if (dob) {
        let date = new Date(dob);
        let day = date.getDate();
        day = day > 10 ? day : `0${day}`;
        let month = date.getMonth() + 1;
        month = month > 10 ? month : `0${month}`;
        let year = date.getFullYear();
        return `${day}/${month}/${year}`;
      }
    }
  },
  setup(props) {
    const { proxy } = getCurrentInstance();
    window.tr = proxy;
    const data = ref(props.value);

    onMounted(() => {
      watch(
        () => data.value,
        (newVal) => {
          proxy.$emit("update:value", newVal);
        }
      );

      watch(
        () => proxy.value,
        (newVal) => {
          proxy.data = newVal;
        }
      );
    });

    const setTooltipDisplay = () => {
      let offset = proxy.$refs.td.getBoundingClientRect();
      if (proxy.text.length > offset.width) {
        return proxy.text;
      } else return "";
    };

    const handleClickOption = () => {
      proxy.isShowOption = true;
    }
    const handleClickUpdate = (e, item) => {
      proxy.eventBus.emit("sendDataEmp", item)
    };
    const handleClickDelete = (e, item) => {
      proxy.eventBus.emit("senDataEmpDelete", item)
    }
    const closeOption = () => {
      proxy.isShowOption = false;
    }
    const styles = computed(() => {
      let arr = [];
      if (props.config.width) {
        arr.push("width: " + props.config.width + "px;");
        arr.push("min-width: " + props.config.width + "px;");
      }
      if (props.config.minWidth) {
        arr.push("min-width: " + props.config.minWidth + "px;");
      }

      return arr.join("; ");
    });

    const showValue = () => {
      let rs = props.value;

      switch (props.config.type) {
        case ColumnType.Text:
          rs = props.value || "";
          break;
        case ColumnType.Number:
          // rs = commonFunction.formatNumber(props.value);
          break;
      }

      return rs;
    };

    const text = computed(() => showValue());

    const cls = computed(() => {
      let rs = [];

      switch (props.config.type) {
        case ColumnType.Checkbox:
          rs.push("text-center");
          break;
        case ColumnType.Number:
          rs.push("text-right");
          break;
      }

      return rs.join(" ");
    });
    const styleAlign = computed(() => {
      let rs = [];

      switch (props.config.align) {
        case ColumnType.AlignCenter:
          rs.push("text-align__center");
          break;
        case ColumnType.AlignLeft:
          rs.push("text-align__left");
          break;
        case ColumnType.AlignRight:
          rs.push("text-align__right");
          break;
      }

      return rs.join(" ");
    });

    return {
      text,
      ColumnType,
      styles,
      cls,
      data,
      setTooltipDisplay,
      styleAlign,
      handleClickOption,
      handleClickUpdate,
      closeOption,
      handleClickDelete
    };
  },
  data() {
    return {
      isShowOption: false,
    };
  },
};
</script>
  
<style lang="scss" scope="">
.sticky_body_right {
  position: sticky;
  right: 0px;
  background-color: #fff;
}

.edit-option {
  display: flex;
  justify-content: center;
  align-items: center;

  button {
    border: unset;
    background-color: #fff;
    cursor: pointer;
  }

  button:hover,
  button:focus {
    background-color: #eceef1;
  }

}

.text-edit {
  cursor: pointer;
  color: #0075c0;
  margin-right: 10px;
  font-weight: 700;
}

.dlg-option {
  width: 40px;
  text-align: center;
  border-radius: 2px;
  z-index: 5;
  background-color: #fff;
  position: absolute;
  border: 1px solid #babec5;
}

.action-group {
  display: flex;
  justify-content: center;

  .icon {
    cursor: pointer;
    margin: 0 4px;
  }
}

.th-inner {
  font-size: 14px;
}

.td-inner {
  font-size: 14px;
  max-width: 248px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

input[type="checkbox"] {
  width: 14px;
  height: 14px;
}

.ms-tr {
  .text-center {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 100%;

    &:first-child {
      margin-left: -3px;
    }
  }

  .text-right {
    &:first-child {
      display: flex;
      align-items: center;
      justify-content: center;
      text-align: center;
    }
  }
}

/* Fixed table */
table tbody td:first-child {
  position: sticky;
  left: 0px;
  z-index: 1;
  background-color: #fff;
  width: 50px;
}

table tbody td:nth-child(2) {
  position: sticky;
  left: 48px;
  z-index: 1;
  background-color: #fff;
  text-align: left;
}

table tbody td:nth-child(3) {
  position: sticky;
  left: 170px;
  z-index: 1;
  background-color: #fff;
  text-align: left;
}

table td:nth-child(4) {
  width: 40px;
}

table tbody td:last-child {
  position: sticky;
  right: 0px;
  z-index: 1;
  background-color: #fff;
}


.text-align__center {
  text-align: center;
}

.option-delete {
  z-index: 5;
}
</style>
  