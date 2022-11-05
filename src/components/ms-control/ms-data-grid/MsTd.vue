<template>
  <td :style="styles" :class="[cls, styleAlign]">
    <div class="td-inner" ref="td">
      <template v-if="config.type == ColumnType.Checkbox">
        <ms-checkbox v-model="data"></ms-checkbox>
      </template>

      <template v-else-if="config.type == ColumnType.Action">
        <div class="edit-option">
          <div class="text-edit">Sửa</div>
          <button class="icon arrow-up--blueicon hw-16">
          </button>
        </div>
        <div v-show="isShowOption && itemSelected.EmployeeId == employee.EmployeeId" class="dlg-option"
          v-if="this.isShowOption == true" v-click-away="closeOption">
          <div @click="deleteEmployee" class="option option-delete">Xoá</div>
        </div>
      </template>

      <template v-else-if="config.align == ColumnType.AlignCenter">
        {{ text }}
      </template>
      <template v-else> {{ text }} </template>
    </div>
  </td>
</template>
  
<script>
import MsCheckbox from "@/components/ms-control/ms-check-box/MsCheckBox.vue";
import {
  computed, getCurrentInstance,
  onMounted,
  ref,
  watch
} from "vue";


const ColumnType = {
  Text: 'Text',
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
      default: {},
    },
    value: {
      default: null,
    },
  },
  methods: {},
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

}

.text-edit {
  cursor: pointer;
  color: #0075c0;
  margin-right: 10px;
  font-weight: 700;
}

.dlg-option {
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
  font-size: 13px;
}

.td-inner {
  font-size: 13px;
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

.text-align__center {
  text-align: center;
}
</style>
  