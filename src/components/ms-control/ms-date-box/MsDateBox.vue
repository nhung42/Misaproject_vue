<template>
    <div class="input-date__field">
        <label class="text-label" v-if="hasLabel" :for="id">
            {{ label ? label : "" }}
            <span v-if="hasInput">*</span>
        </label>
        <div class="flex-row" :class="[leftIcon ? 'has-icon' : '']">
            <date-picker :id="id || ''" class="input-text" v-model="dateValue" format="dd/MM/yyyy" :tabindex="tabindex">
            </date-picker>
        </div>
        <span v-if="disabledMessage" class="error-message">{{
                message ? message : ""
        }}</span>
    </div>
</template>

<script>
import {
    defineComponent,
    ref,
    watch,
    nextTick,
    getCurrentInstance,
    onMounted,
} from "vue";
import DatePicker from "vuejs3-datepicker";
import { defaultDateProps } from './DefaultProps';

export default defineComponent({
    name: "msDateBox",
    components: { DatePicker },
    props: defaultDateProps,
    setup(props, { emit }) {
        const { proxy } = getCurrentInstance();
        window.ipdate = proxy;
        const dateValue = ref("");

        watch(
            () => proxy.modelValue,
            (newVal) => {
                proxy.dateValue = newVal;
            }
        );

        const changeValue = function () {
            proxy.$emit("update:modelValue", proxy.dateValue);
            nextTick(() => {
                emit("changeValue", proxy.dateValue, proxy.valueField);
            });
        };

        onMounted(() => {
            proxy.changeValue();
        });

        return { dateValue, changeValue };
    }
});
</script>
<style lang="scss" scope="">
@import "@/style/components/MsInputDate.scss";
</style>
  