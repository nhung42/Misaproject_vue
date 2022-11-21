<template>
    <div class="grid-container">
        <div class="grid-view">
            <div class="div-table">
                <table>
                    <thead>
                        <tr>
                            <th v-if="selectedCol">
                                <div class="th-inner">
                                    <ms-checkbox v-model="allSelected"></ms-checkbox>
                                </div>
                            </th>

                            <th ref="th" v-for="col in columns" :key="col" :config="col">
                                <ms-tooltip
                                    :content="col.title == resourceTable.lblTableEmployee.lblIdentityNumber ? 'Số Chứng Minh Nhân Dân' : ''"
                                    placement="bottom" right="bottom">
                                    {{ col.title }}
                                </ms-tooltip>
                            </th>
                        </tr>
                    </thead>
                    <tbody>
                        <ms-tr v-for="(item, i) in this.employeeData" :key="item.EmployeeId" :data="item"
                            :columns="columns" :selectedCol="selectedCol" v-model="selectedIndex[i]"
                            @click="handleClick(i)" @dblclick="handleDoubleClick(item)">
                            {{ item.EmployeeId }}
                        </ms-tr>
                    </tbody>
                </table>
            </div>
            <div class="paging">
                <div class="paging-left"> Tổng:
                    <p> <span>{{ this.toalRecord }}</span></p>
                    &nbsp;bản ghi
                </div>
                <div class="paging-right">
                    <div class="paging-page-limit">
                        <span>Số bản ghi/ trang</span>
                        <div class="total-page">
                            <ms-select :id="'pagingSize'" class="pagingOption" :data="pageLimit" :width="'60'"
                                :iconLagre="true" @getSelectValue="getLimit">
                            </ms-select>
                        </div>
                    </div>
                    <div class="paging-page-range">
                        Từ {{ pageUpdate(toalRecord) }} bản ghi
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
        <ms-employee-popup v-if="isShowPopup" :formModel="pram" :dataPram="pramData" @closePopup="handlClosePopup">
        </ms-employee-popup>
    </div>
    <!-- Dialog xóa 1 dòng -->
    <teleport to="body">
        <ms-message-box :disabledTop="false" :title="Resource.TitleDialog.DeleteOneEmployee" leftIcon="icon-warring"
            :valueMessageBox="valueMessageBox" :textMessageBox="Resource.TitleDialogMessage.DeleteOneEmployee.VI"
            :disabledValueLeft="false" :disabledValueRight="true" v-if="isDialogMessDelete">
            <ms-button :text="Resource.TitleBtnDialog.Delete.VI" @click="deleteEmployee(employeeId)" radius></ms-button>
            <ms-button :text="Resource.TitleBtnDialog.NoCancel.VI" type="secodary" @click="isDialogMessDelete = false"
                radius>
            </ms-button>
        </ms-message-box>
    </teleport>

    <!-- Dialog cancel xóa -->
    <teleport to="body">
        <ms-message-box :disabledTop="false" leftIcon="icon-warring" :valueMessageBox="valueMessageBox"
            :textMessageBox="Resource.TitleDialogMessage.CancelDelete.VI" :disabledValueLeft="false"
            :disabledValueRight="false" v-if="isDialogMessCancelDelete">
            <ms-button :text="Resource.TitleBtnDialog.Close.VI" radius></ms-button>
        </ms-message-box>
    </teleport>
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
    onBeforeUnmount,
    // onUpdated,
    defineComponent,
} from "vue";
import MsTr from "./MsTr.vue";
import Enum from "@/dictionary/enum.js";
import MsCheckbox from "@/components/ms-control/ms-check-box/MsCheckBox.vue";
import MsEmployeePopup from "@/views/employee/EmployeePopup.vue";
import MsButton from "../ms-button/MsButton.vue";
import MsSelect from "../ms-select-box/MsSelectBox.vue";
import MsMessageBox from "@/components/dialog/MsMessageBox.vue";
import Resource from "@/dictionary/resource";
import resourceTable from "@/dictionary/resourceTable";
import MsTooltip from "../tooltip/MsTooltip.vue";
import axios from 'axios';

export default defineComponent({
    name: "MsGrid",
    components: { MsTr, MsCheckbox, MsEmployeePopup, MsSelect, MsMessageBox, MsButton, MsTooltip },
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
        search: {
            type: String,
            default: "",
        },
        reloadData: {
            type: Boolean,
            default: false,
        }
    },
    mounted() {
        //Thực thi load  dữ liệu
        this.loadData()
    },
    watch: {
        //Tìm kiếm và load lại dữ liệu
        search() {
            this.pageNumber = 1;
            this.loadData();
        },
        //Load lại dữ liệu
        reloadData() {
            this.pageNumber = 1;
            this.loadData();
        }
    },
    methods: {
        /**
         * Gán giá trị pageSize được chọn
         */
        getLimit(limit) {
            this.page.limit = limit;
            this.loadData();
        },
        /**
         * Gọi Api lấy tất cả nhân viên và phân trang tìm kiếm
         * @uthor DuongNhung
         */
        loadData() {
            try {
                axios
                    .get(`https://amis.manhnv.net/api/v1/Employees/filter?pageSize=${this.page.limit}&pageNumber=${this.pageNumber}&employeeFilter=${this.search}`)
                    .then(response => {
                        this.employeeData = response.data?.Data;
                        this.employeeData = this.employeeData.map(x => {
                            x.IsShowDelete = false;
                            return x;
                        })
                        this.toalRecord = response.data.TotalRecord;
                    })
                    .catch(function (error) {
                        this.error = error.response.data.devMsg;

                    });

            } catch (error) {
                console.log(error);
            }

        },
        /**
         * Hàm gán giá trị pageNumber cho trang tiếp theo
         * @author DuongNhung
         */
        nextPage() {
            try {
                if (Math.ceil(this.toalRecord / this.page.limit) > this.pageNumber) {
                    this.pageNumber++
                    this.loadData();
                }
            } catch (error) {
                console.log(error);
            }
        },
        /**
       * Hàm gán giá trị pageNumber cho trang trước đó
       * @author DuongNhung
       */
        prevPage() {
            try {
                if (this.pageNumber > 1) {
                    this.pageNumber--;
                    this.loadData();
                }
            } catch (error) {
                console.log(error);
            }
        },
        /**
         * Hàm đưa ra giá trị thứ tự bản ghi lấy ra của mỗi trang
         * @param {*} total 
         * @author DuongNhung
         */
        pageUpdate(total) {
            try {
                return `${this.page.limit * (this.pageNumber - 1) + 1} - ${this.page.limit * this.pageNumber > total
                    ? total
                    : this.page.limit * this.pageNumber
                    } `;
            } catch (error) {
                console.log(error);
            }
        }
    },
    setup(props, { emit }) {
        const { proxy } = getCurrentInstance();
        //State được chọn
        const selected = ref([]);
        //State show popup
        const isShowPopup = ref(false);
        //State show dialog muốn xóa 
        const isDialogMessDelete = ref(false);
        //State show dialog hủy xóa
        const isDialogMessCancelDelete = ref(false);
        const employeeId = ref("");
        window.tables = proxy;
        let pram = reactive({
            mode: 0,
            EmployeeId: "",
        });
        let pramData = ref({});
        const allSelected = ref(false);
        // Lấy ra những vị trí checked
        const selectedIndex = ref([]);
        /**Lấy ra dữ liệu các vị trí được chọn */
        const dataSelected = computed(() =>
            selectedIndex.value.map((x, i) => x && proxy.allData[i] && proxy.allData[i].EmployeeId).filter((x) => x)
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
         * Hàm xử lí gọi dialog cảnh báo xoá
         * @param {*} id 
         * @author DuongNhung
         */
        const handleDeleteData = function (id) {
            proxy.employeeId = id;
            proxy.isDialogMessDelete = true;
        }
        /**
       * Call api xoá thông tin nhân viên
       * @param {*} 
       * @author DuongNhung
       */
        const deleteEmployee = (employeeId) => {
            try {
                axios
                    .delete(`https://amis.manhnv.net/api/v1/Employees/${employeeId}`)
                    .then(() => {
                        proxy.isDialogMessDelete = !proxy.isDialogMessDelete;
                        proxy.loadData();
                    })
                    .catch(function (error) {
                        proxy.error = error.response.data.devMsg;

                    })
            } catch (error) {
                console.log(error);
            }
        };
        /**
        * Xử lý sự kiện double click tr
        *  @author DuongNhung
        * @pram {object} item dữ liệu employeehandlClosePopup khi click tr
        */
        const handleDoubleClick = (item) => {
            proxy.pram.mode = Enum.Mode.Update;
            proxy.pramData = item;
            proxy.isShowPopup = true;
        };
        /**
         * Xử lí sự kiện đóng popup
         * @param {*} 
         * @author DuongNhung
         */
        const handlClosePopup = () => {
            proxy.isShowPopup = false;
        };

        /**
        * Xử lý sự kiện bỏ checked 1 dòng 
        *  @author DuongNhung
        */
        const handleClick = (index) => {
            proxy.employeeData = proxy.employeeData.map((x, a) => {
                if (a == index) {
                    x.IsShowDelete = !x.IsShowDelete;
                }
                else {
                    x.IsShowDelete = false;
                }
                return x;
            })
            if (proxy.selectedIndex[index]) {
                proxy.selectedIndex[index] = false;
            } else {
                proxy.selectedIndex[index] = true;
            }
        };
        /**
         * Nhận giá trị item và gán data vào form Update
         * @param {*} item 
         * @author DuongNhung
         */
        const handleEmitData = function (item) {
            console.log(item)
            proxy.pram.mode = Enum.Mode.Update;
            proxy.pramData = item;
            proxy.isShowPopup = true;
        }
        /**
         * Nhận giá trị item và gán data vào form Update
         * @param {*} item 
         * @author DuongNhung
         */
        const handlEmitDataDuplicate = function (item) {
            console.log(item)
            proxy.pram.mode = Enum.Mode.Duplicate;
            proxy.pramData = item;
            proxy.isShowPopup = true;
        }
        onMounted(() => {
            proxy.eventBus.on("sendDataEmp", handleEmitData);
            proxy.eventBus.on("senDataEmpDelete", handleDeleteData);
            proxy.eventBus.on("sendDataDuplicateEmp", handlEmitDataDuplicate);
        });
        onBeforeUnmount(() => {
            proxy.eventBus.off("sendDataEmp");
            proxy.eventBus.off("senDataEmpDelete");
            proxy.eventBus.off("sendDataDuplicateEmp");
        });
        return {
            selected,
            dataSelected,
            handleDoubleClick,
            Resource,
            resourceTable,
            pram,
            isShowPopup,
            allSelected,
            selectedIndex,
            handleClick,
            pramData,
            handlClosePopup,
            isDialogMessDelete,
            isDialogMessCancelDelete,
            deleteEmployee,
            employeeId,

        };

    },
    data() {
        return {
            toalRecord: 0,
            pageNumber: 1,
            page: {
                limit: 10,
                offset: 0,
            },
            error: "",
            pageLimit: [{ value: 10 }, { value: 20 }, { value: 50 }, { value: 100 }],
            employeeData: {},
        };
    },

});
</script>
<style lang="scss" scoped>
@import "@/style/components/MsGrid.scss";
@import "@/style/common/icon.scss";

.pagingOption {
    width: 75px;
    border-radius: 4px;
    padding: 8px;
    font-weight: bold;
    display: flex;
    justify-content: center;
}

.pagingOption .option {
    top: -170px;
}
</style>
  