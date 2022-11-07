<template>
    <div class="grid-container">
        <div class="grid-view">
            <div class="div-table">
                <table>
                    <thead>
                        <tr>
                            <th v-if="selectedCol" style="width: 47px">
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
                    <p> <span>{{ allData.length }}</span></p>
                    &nbsp;bản ghi
                </div>
                <div class="paging-right">
                    <div class="paging-page-limit">
                        <span>Số bản ghi/ trang</span>
                        <div class="total-page">
                            <ms-select :id="'pagingSize'" class="pagingOption" :data="pageLimit" :width="'100'"
                                :iconLagre="true" @getSelectValue="getLimit"></ms-select>
                        </div>
                    </div>
                    <div class="paging-page-range">
                        Từ <b>{{ from }}</b> đến <b>{{ to }}</b> bản ghi
                    </div>
                    <div class="paging__page-button">
                        <button class="btn--prev" @click="prevPage">
                            <div class="icon-arrow-left"></div>
                        </button>
                        <button class="btn--next" @click="nextPage">
                            <div class="icon-arrow-right"></div>
                        </button>
                    </div>
                </div>
            </div>
        </div>
        <ms-employee-popup v-if="isShowPopup" :formModel="pram" :dataPram="pramData"></ms-employee-popup>
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
import MsEmployeePopup from "@/views/employee/EmployeePopup.vue";
// import MsDropdown from "@/components/ms-control/dropdown/MsDropdown.vue";
import MsSelect from "../ms-select-box/MsSelectBox.vue";

export default defineComponent({
    name: "MsGrid",
    components: { MsTr, MsCheckbox, MsEmployeePopup, MsSelect },
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
        tableName: {
            type: Array,
            default: () => [],
        },
    },
    methods: {
        getLimit(limit) {
            this.page.limit = limit;
        },
        pagiagte(arr) {
            let page = this.pageNumber;
            let perPage = this.page.limit;
            let from = page * perPage - perPage;
            let to = page * perPage;
            return arr.slice(from, to);
        },
        nextPage() {
            if (this.pageNumber == this.maxPage) {
                this.pageNumber = 1;
                return;
            }
            this.pageNumber++;
        },
        prevPage() {
            if (this.pageNumber == 1) {
                this.pageNumber = this.maxPage;
                return;
            }
            this.pageNumber--;
        },
    },
    computed: {
        displayData() {
            return this.pagiagte(this.tableName);
        },
        from() {
            var from = this.page.limit * this.pageNumber - this.page.limit + 1;
            if (this.tableName.length == 0) return 0;
            if (from < 10 && from > 0) return "0" + from;
            return from;
        },
        to() {
            var to = this.page.limit * this.pageNumber;
            if (to > this.tableName.length) to = this.tableName.length;
            if (to < 10 && to > 0) return "0" + to;
            return to;
        },
        maxPage() {
            return Math.ceil(this.tableName.length / this.page.limit);
        },
    },
    setup(props, { emit }) {
        const { proxy } = getCurrentInstance();
        const selected = ref([]);
        const isShowPopup = ref(false);
        window.tables = proxy;
        let pram = reactive({
            mode: 0,
            EmployeeId: "",
        });
        // let pramData = ref({});

        const allSelected = ref(false);
        // Lấy ra những vị trí checked
        const selectedIndex = ref([]);
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
        });
        return {
            selected,
            dataSelected,
            handleDoubleClick,
            pram,
            isShowPopup,
            allSelected,
            selectedIndex,
            handleClick,
        };
    },
    data() {
        return {
            pageNumber: 1,
            page: {
                limit: 20,
                offset: 0,
            },
            pageLimit: [{ value: 10 }, { value: 20 }, { value: 50 }, { value: 100 }],
        };
    },

});
</script>
<style lang="scss" scoped>
@import "@/style/components/MsGrid.scss";
@import "@/style/common/icon.scss";

.pagingOption {
    width: 75px;
    border: 1px solid #e0e0e0;
    border-radius: 4px;
    padding: 8px;
    font-weight: bold;
    display: flex;
    justify-content: center;
}

.pagingOption .option {
    top: -170px;
}

.active-tr {
    background-color: rgb(236, 238, 241);
}
</style>
  