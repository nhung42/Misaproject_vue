<template>
    <label for="" class="input-checkbox">
        <input type="checkbox" class="checkbox-control" v-model="isValue" @change="changeValue" :readonly="readonly"
            :disabled="disabled" :id="id == null ? '' : id" />
    </label>
</template>
<script>
import {
    getCurrentInstance,
    ref,
    watch,
    // resolveComponent as _resolveComponent,
    // mergeProps as _mergeProps,
} from "vue";
export default {
    name: "MsCheckbox",
    props: {
        modelValue: {
            default: false,
            type: [Boolean, Number],
        },
        readonly: {
            default: false,
            type: Boolean,
        },
        disabled: {
            default: false,
            type: Boolean,
        },
        id: {
            default: null,
            type: String,
        },
        checked: {
            type: [Boolean, Number],
            default: false,
        },
    },
    setup() {
        const { proxy } = getCurrentInstance();
        const isValue = ref(false);
        /**
         * Thay đổi giá trị chọn
         * @param {*} 
         * @author DuongNhung
         */
        const changeValue = function () {
            proxy.$emit("update:modelValue", proxy.isValue);
        };

        watch(
            () => proxy.modelValue,
            (val) => {
                proxy.isValue = val;
            }
        );

        return {
            isValue,
            changeValue,
        };
    },
};
</script>
<style lang="scss" scoped>
@import "@/style/components/MsCheckbox.scss";
</style>
  