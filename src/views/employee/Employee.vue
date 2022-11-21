<template>
    <div class="ms-content">
        <div class="contrainer-toolbar">
            <div class="page-title">
                Nhân viên
            </div>
            <ms-tooltip content="Thêm mới nhân viên" placement="bottom" right="bottom">
                <ms-button ref="MsPopupEmployee" text="Thêm mới nhân viên" id="btn-add" :radius="true"
                    @click="handleClickAdd">
                </ms-button>
            </ms-tooltip>
            <ms-popup-employee v-if="isShowPopup" :formModel="pram" @closePopup="handlClosePopup">
            </ms-popup-employee>
        </div>
        <div class="page__toolbar">
            <ms-input :hasLabel="false" leftIcon="ic-search" id="txt-search" :radius="true" placeholder="Tìm kiếm "
                :disabledMessage="false" message="" class="input_search" v-model="search"></ms-input>
            <ms-tooltip content="Lấy lại dữ liệu" placement="bottom" right="bottom">
                <div class="icon-reload margin-left-10px" @click="loadData"></div>
            </ms-tooltip>
        </div>
        <ms-grid :columns="columns" :selectedCol="true" v-model="dataSelected" :search="search" :reloadData="reload">
        </ms-grid>
    </div>

</template>

<script>
import MsGrid from "@/components/ms-control/ms-data-grid/MsDataGrid.vue";
import MsButton from "@/components/ms-control/ms-button/MsButton.vue";
import MsInput from "@/components/ms-control/ms-text-box/MsTextBox.vue";
import MsTooltip from "@/components/ms-control/tooltip/MsTooltip.vue";
import Enum from "@/dictionary/enum.js";
import MsPopupEmployee from "@/views/employee/EmployeePopup.vue";
import {
    getCurrentInstance,
    reactive,
    ref
} from "vue";
export default {
    name: "MsEmployee",
    components: {
        MsButton,
        MsInput,
        MsGrid,
        MsTooltip,
        MsPopupEmployee,
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
        const reload = ref(false);

        let pram = reactive({
            mode: 0,
            employeeId: "",
        });

        /**
         * Xử lý sự kiện click thêm mới
         *  @author DuongNhung
         */
        const handleClickAdd = () => {
            proxy.pram.mode = Enum.Mode.Add;
            proxy.isShowPopup = true;
        };
        /**
         * Xử lí sự kiện đóng popup
         * @author DuongNhung
         */
        const handlClosePopup = () => {
            proxy.isShowPopup = false;
        };
        /**
         * set giá trị hàm loadData gọi từ data grid
         */
        const loadData = () => {
            proxy.reload = !proxy.reload;
        };

        const showToastMessage = () => {
            if (proxy.pram.mode == 1) {
                proxy.isShowMessage = true;
            }
            if (proxy.pram.mode == 2) {
                proxy.isShowMessageUpdate = true;
            }
        };
        const columns = ref([
            {
                id: 1,
                field: "EmployeeCode",
                title: "Mã nhân viên",
                type: "Text",
                width: 110,
            },
            {
                id: 2,
                field: "EmployeeName",
                title: "Họ và tên",
                type: "Text",
                width: 140,
            },
            {
                id: 3,
                field: "GenderName",
                title: "Giới tính",
                type: "Text",
                width: 40,
            },
            {
                id: 4,
                field: "DateOfBirth",
                title: "Ngày sinh",
                type: "Date",
                width: 100,
            },
            {
                id: 5,
                field: "IdentityNumber",
                title: "Số CMND",
                type: "Text",
                width: 100,
            },
            {
                id: 6,
                field: "PositionName",
                title: "Chức Danh",
                type: "Text",
                width: 110,
            },
            {
                id: 7,
                field: "DepartmentName",
                title: "Tên đơn vị",
                type: "Text",
                width: 110,
            },
            {
                id: 8,
                field: "BankAccountNumber",
                title: "Số Tài khoản",
                type: "Text",
                width: 110,
            },
            {
                id: 9,
                field: "BankName",
                title: "Tên ngân hàng",
                type: "Text",
                width: 110,
            },
            {
                id: 10,
                field: "BankBranchName",
                title: "Chi nhánh TK Ngân hàng",
                type: "Text",
                width: 160,
            },
            {
                id: 11,
                field: "",
                title: "Chức năng",
                type: "Action",
                width: 80,
            },
        ]);

        return {
            columns,
            allData,
            dataSelected,
            isLoading,
            Loading,
            handlClosePopup,
            pram,
            loadData,
            handleClickAdd,
            reload,
            showToastMessage
        };
    },
    data() {
        return {
            isShowPopup: false,
            search: "",
        };
    },
};
</script>

<style lang="scss" >
.ic-search {
    background-image: url(/src/assets/img/icon_search.png);
    width: 5px;
    height: 16px;
    margin-top: -8px !important;
}

@import "@/style/layout/Employee.scss";
@import "@/style/components/MsMessageBox.scss";
</style>
  