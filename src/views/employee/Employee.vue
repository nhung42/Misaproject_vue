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
            <ms-popup-employee v-if="isShowPopup" :formModel="pram" @closePopup="handlClosePopup"></ms-popup-employee>
        </div>
        <div class="page__toolbar">
            <ms-input :hasLabel="false" leftIcon="ic-search" id="txt-search" :radius="true" placeholder="Tìm kiếm "
                :disabledMessage="false" message="" class="input_search"></ms-input>
            <ms-tooltip content="Lấy lại dữ liệu" placement="bottom" right="bottom">
                <div class="icon-reload margin-left-10px" @click="loadData"></div>
            </ms-tooltip>
        </div>

        <ms-grid :columns="columns" :allData="allData.value" :selectedCol="true" v-model="dataSelected">
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
import axios from 'axios';
import {
    getCurrentInstance,
    onMounted,
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
        let pram = reactive({
            mode: 0,
            employeeId: "",
        });
        async function loadData() {
            try {
                proxy.isLoading = true;
                await axios
                    .get('https://amis.manhnv.net/api/v1/Employees/filter')
                    .then(response => {
                        let data = response.data?.Data;
                        proxy.allData.value = data;
                    })
                proxy.isLoading = false;
            } catch (error) {
                console.log(error);
            }
        }

        onMounted(() => {
            (async () => {
                await proxy.loadData();
            })();
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
        const clickMenu = async (action, val) => {
            switch (action) {
                case 0: {
                    proxy.pram.mode = Enum.Mode.Update;
                    proxy.pram.employeeId = val;
                    proxy.isShowPopup = true;
                    break;
                }
            }
        };
        const columns = ref([
            {
                field: "EmployeeCode",
                title: "Mã nhân viên",
                type: "Text",
                width: 110,
            },
            {
                field: "EmployeeName",
                title: "Họ và tên",
                type: "Text",
                width: 180,
            },
            {
                field: "GenderName",
                title: "Giới tính",
                type: "Text",
                width: 40,
            },
            {
                field: "DateOfBirth",
                title: "Ngày sinh",
                type: "Date",
                width: 100,
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
            clickMenu,
            handlClosePopup,
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
  