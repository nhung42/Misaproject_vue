<template>
    <div class="grid-container">
        <div class="grid-view">
            <div class="div-table">
                <table>
                    <thead>
                        <tr>
                            <th v-if="selectedCol" style="width: 50px">
                                <div class="th-inner">
                                    <ms-checkbox v-model="allSelected"></ms-checkbox>
                                </div>
                            </th>

                            <th ref="th" v-for="col in columns" :key="col" :config="col">
                                {{ col.title }}
                            </th>
                        </tr>
                    </thead>
                    <tbody>
                        <ms-tr v-for="(item, i) in allData" :key="item.EmployeeId" :data="item" :columns="columns"
                            :selectedCol="selectedCol" v-model="selectedIndex[i]" @click="handleClick(i)"
                            @dblclick="handleDoubleClick(item)">
                            {{ item.EmployeeId }}
                        </ms-tr>
                    </tbody>
                </table>
            </div>
            <div class="paging">
                <div class="paging-left"> Tổng:
                    <span>4</span>
                </div>
                <div class="paging-right">
                    <div class="paging-page-limit">
                        <span>Số bản ghi/ trang</span>
                        <div class="total-page">
                            <select>
                                <option class="item-total" value="20" selected>20</option>
                                <option class="item-total" value="50">50</option>
                                <option class="item-total" value="100">100</option>
                                <option class="item-total" value="200">200</option>
                            </select>
                            <div class="icon-bottom app-icon icon-combobox-pagging"></div>
                        </div>
                    </div>
                    <div class="paging-page-range">
                        Từ 1- 40 bản ghi
                    </div>
                    <div class="paging__page-button">
                        <button class="btn--prev">
                            <div class="icon-arrow-left"></div>
                        </button>
                        <button class="btn--next">
                            <div class="icon-arrow-right"></div>
                        </button>
                    </div>
                </div>
            </div>
        </div>
        <!-- <ms-popup-asset v-if="isShowPopup" :formModel="pram" :dataPram="pramData"></ms-popup-asset> -->
    </div>
</template>
<script>
import {
    getCurrentInstance,
    onMounted,
    ref,
    watch,
    reactive,
    nextTick,
    computed,
    // onUpdated,
    defineComponent,
} from "vue";
import MsTr from "./MsTr.vue";
import Enum from "@/dictionary/enum.js";
import MsCheckbox from "@/components/ms-control/ms-check-box/MsCheckBox.vue";
import MsDropdown from "../dropdown/MsDropdown.vue";

export default defineComponent({
    name: "MsGrid",
    components: { MsTr, MsCheckbox, MsDropdown },
    props: {
        selectedCol: {
            default: false,
            type: Boolean,
        },
        columns: {
            default: [],
        },
        allData: {
            default: [],
        },
        idField: {
            default: "",
        },
        modelValue: {
            default: [],
        },
        selectedField: {
            default: "",
        },
        selectedData: {
            default: [],
        },
    },

    setup(props, { emit }) {
        const { proxy } = getCurrentInstance();
        const selected = ref([]);
        const isShowPopup = ref(false);
        window.tables = proxy;
        let pram = reactive({
            mode: 0,
            fixed_asset_id: "",
        });
        // let pramData = ref({});

        const allSelected = ref(false);
        // Lấy ra những vị trí checked
        const selectedIndex = ref([]);
        const dataPageging = ref([1, 2, 4, 3]);
        const dataSelected = computed(() =>
            selectedIndex.value.map((x, i) => x && proxy.allData[i]).filter((x) => x)
        );

        // Cập nhật dataSelected vào selectedData
        onMounted(() => {
            watch(
                () => proxy.dataSelected,
                (newVal) => {
                    emit("update:selected", newVal);
                    nextTick(() => {
                        emit("update:selectedData", proxy.dataSelected);
                    });
                }
            );
        });

        //Theo dõi biến allSelected kiểm tra là true thì lấy tất cả dữ liệu
        watch(
            () => allSelected.value,
            (newVal) => {
                if (newVal) {
                    proxy.selectedIndex = proxy.allData.map((x) => x);
                } else {
                    proxy.selectedIndex = [];
                }
            }
        );

        /**
        * Xử lý sự kiện double click tr
        *  @author DuongNhung
        * @pram {object} item dữ liệu asset khi click tr
        */
        const handleDoubleClick = (item) => {
            proxy.pram.mode = Enum.Mode.Update;
            proxy.pramData = item;
            proxy.isShowPopup = true;
        };

        /**
        * Xử lý sự kiện bỏ checked 1 dòng 
        *  @author DuongNhung
        */
        const handleClick = (index) => {
            if (proxy.selectedIndex[index]) {
                proxy.selectedIndex[index] = false;
            } else {
                proxy.selectedIndex[index] = true;
            }
        };

        onMounted(() => {
            // proxy.handleSum();
        });
        return {
            selected,
            // handleSum,
            dataSelected,
            handleDoubleClick,
            dataPageging,
            pram,
            isShowPopup,
            allSelected,
            selectedIndex,
            handleClick,
        };
    },
});
</script>
<style lang="scss" scoped>
@import "@/style/components/MsGrid.scss";
@import "@/style/common/icon.scss";


.active-tr {
    background-color: rgb(236, 238, 241);
}
</style>
  