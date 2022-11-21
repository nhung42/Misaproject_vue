<template>
    <tr class="ms-tr" style="height: 48px !important;">
        <ms-td v-if="selectedCol" :config="configSelect" v-model="select">
        </ms-td>
        <ms-td v-for="col in columns" :key="col.id" :config="col" :value="data[col.field]" :emp="data">
        </ms-td>
    </tr>
</template>
<script>
import {
    getCurrentInstance,
    onMounted,
    ref,
    reactive,
    watch,
} from "vue";
import MsTd from "@/components/ms-control/ms-data-grid/MsTd.vue";

export default {
    name: "MsTr",
    props: {
        selected: {
            default: false,
            type: Boolean,
        },
        selectedCol: {
            default: false,
            type: Boolean,
        },

        columns: {
            default: [],
        },

        data: {
            default: {},
        },
    },
    components: {
        MsTd,
    },
    setup(props, { emit }) {
        const { proxy } = getCurrentInstance();
        !window.mk && (window.mk = proxy);

        window.tr = proxy;
        const select = ref(props.selected);
        /**
         * Khai báo đối tượng checkbox
         */
        const configSelect = reactive({
            field: "selected",
            title: "checkbox",
            type: "Checkbox",
            width: 50,
        });

        onMounted(() => {
            watch(
                () => proxy.select,
                (newVal) => {
                    emit("update:selected", newVal);
                }
            );

            watch(
                () => proxy.selected,
                (val) => {
                    proxy.select = val;
                }
            );
        });

        return {
            configSelect,
            select,
        };
    },
};
</script>
<style lang="scss" scoped>

</style>
  