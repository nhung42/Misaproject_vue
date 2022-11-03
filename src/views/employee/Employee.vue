<template>
    <div class="contrainer-toolbar">
        <div class="page-title">
            Nhân viên
        </div>
        <ms-tooltip content="Thêm mới nhân viên" placement="bottom" right="bottom">
            <ms-button ref="MsPopupAsset" text="Thêm mới nhân viên" id="btn-add" :radius="true" @click="handleClickAdd">
            </ms-button>
        </ms-tooltip>
        <!-- <ms-popup-asset v-if="isShowPopup" :formModel="pram"></ms-popup-asset> -->
    </div>
    <div class="page__toolbar">
        <ms-input :hasLabel="false" leftIcon="ic-search" id="txt-search" :radius="true" placeholder="Tìm kiếm "
            :disabledMessage="false" message=""></ms-input>
        <ms-tooltip content="Lấy lại dữ liệu" placement="bottom" right="bottom">
            <div class="icon-reload margin-left-10px"></div>
        </ms-tooltip>
    </div>
     
    <ms-grid :columns="columns" :allData="allData.value" :selectedCol="true" v-model="dataSelected">
    </ms-grid>
</template>

<script>
import MsGrid from "@/components/ms-control/ms-data-grid/MsDataGrid.vue";
import MsButton from "@/components/ms-control/ms-button/MsButton.vue";
import MsInput from "@/components/ms-control/ms-text-box/MsTextBox.vue";
import MsTooltip from "@/components/ms-control/tooltip/MsTooltip.vue";
import Enum from "@/dictionary/enum.js";
import axios from 'axios';
import {
  getCurrentInstance,
  onMounted,
  reactive,
  ref
} from "vue";


const columns = ref([
            {
                field: "EmployeeCode",
                title: "Mã nhân viên",
                type: "Text",
                width: 100,
            },
            {
                field: "EmployeeName",
                title: "Họ và tên",
                type: "Text",
                minWidth: 140,
            },
            {
                field: "GenderName",
                title: "Giới tính",
                type: "Text",
                width: 60,
            },
            {
                field: "DateOfBirth",
                title: "Ngày sinh",
                type: "Date",
                width: 120,
            },
            {
                field: "IdentityNumber",
                title: "Số CMND",
                type: "Text",
                width: 100,
            },
            {
                field: "PositionName",
                title: "Chức Danh",
                type: "Text",
                width: 110,
            },
            {
                field: "DepartmentName",
                title: "Tên đơn vị",
                type: "Text",
                width: 110,
            },
            {
                field: "BankAccountNumber",
                title: "Số Tài khoản",
                type: "Text",
                width: 110,
            },
            {
                field: "BankName",
                title: "Tên ngân hàng",
                type: "Text",
                width: 110,
            },
            {
                field: "BankBranchName",
                title: "Chi nhánh TK Ngân hàng",
                type: "Text",
                width: 160,
            },
            {
                field: "",
                title: "",
                type: "Action",
                width: 80,
            },
        ]);


export default {
    name: "MsEmployee",
    components: {
        MsButton,
        MsInput,
        MsGrid,
        // MsLoading,
        MsTooltip,
    },
    methods: {
        close() {
            this.isShowPopup = false;
        },
        hanhdleAccumulated() { },
    },
    async setup() {
        const { proxy } = getCurrentInstance();
        window.asset = proxy;
        //Loading form
        const isLoading = ref(false);

        const allData = ref([]);
        const Loading = ref(true);
        const dataSelected = ref([]);
        let pram = reactive({
            mode: 0,
            fixed_employee_id: "",
        });
        async function loadData() {
            try {
                // proxy.isLoading = true;
                await axios
                    .get('https://amis.manhnv.net/api/v1/Employees/filter')
                    .then(response => {
                        let data = response.data?.Data;
                        proxy.allData.value = data;
                        console.log(allData);
                    })
                // proxy.isLoading = false;
            } catch (error) {
                console.log(error);
            }
        }

        onMounted(() => {
            proxy.loadData();
        });

        
        /**
         * Xử lý sự kiện click thêm mới
         *  @author DuongNhung
         */
        const handleClickAdd = () => {
            proxy.pram.mode = Enum.Mode.Add;
            proxy.isShowPopup = true;
        };
        const clickMenu = async (action, val) => {
            switch (action) {
                case 0: {
                    proxy.pram.mode = Enum.Mode.Update;
                    proxy.pram.fixed_employee_id = val;
                    proxy.isShowPopup = true;
                    break;
                }
                case 1: {
                    proxy.pram.mode = Enum.Mode.Duplicate;
                    proxy.pram.fixed_employee_id = val;
                    proxy.isShowPopup = true;
                    break;
                }
            }
        };

        
        return {
            columns,
            allData,
            dataSelected,
            isLoading,
            Loading,
            clickMenu,
            pram,
            loadData,
            handleClickAdd,
        };
    },

    data() {
        return {
            isShowPopup: false,
        };
    },
};
</script>

<style lang="scss" scoped>
@import "@/style/layout/Employee.scss";
@import "@/style/components/MsMessageBox.scss";
</style>
  