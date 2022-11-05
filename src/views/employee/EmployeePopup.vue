<template>
    <teleport to="body">
        <div class="model">
            <div class="form-employee">
                <div class="header-popup">
                    <div class="dialog__header-left  display-flex">
                        <div class="header__title">
                            Thông tin nhân viên</div>
                        <div class="header__type display-flex margin-left-10px">
                            <div class="checkbox-type">
                                <input type="checkbox" id="customer" name="customer" class="m-checkbox" value="customer"
                                    tabindex="22">
                                <label for="customer"> Là khách hàng</label><br>
                            </div>
                            <div class="checkbox-type">
                                <input type="checkbox" id="supplier" name="supplier" value="supplier" class="m-checkbox"
                                    tabindex="23">
                                <label for="supplier"> Là nhà cung cấp</label><br>
                            </div>
                        </div>
                    </div>
                    <button class="dialog__button display-flex">
                        <div class="icon-help" title="Giúp (F1)"></div>
                        <v-tooltip content="Đóng(ESC)" placement="bottom" right="bottom">
                            <div class="icon-close" @click="handlePopupClose"></div>
                        </v-tooltip>
                    </button>
                </div>
                <div class="content-popup">
                    <div class="employee__body">
                        <div class="employee__left">
                            <div class="m-row display-flex">
                                <div class="input-wrapper width-40">
                                    <v-input label="Mã" tabindex="1" hasLabel hasInput :maxLength="15"
                                        ref="inputEmployeeCode" v-model="dataForm.employeeCode" :radius="false"
                                        placeholder="Mã nhân viên" @blur="changeValueInput" @focus="changeValueInput">
                                    </v-input>
                                </div>
                                <div class="input-wrapper margin-left-10px flex-1">
                                    <v-input label="Họ và tên" tabindex="2" hasLabel hasInput :maxLength="15"
                                        v-model="dataForm.employeeName" :radius="false" placeholder="Tên nhân viên"
                                        @blur="onBlurInput" @focus="changeValueInput"></v-input>
                                </div>
                            </div>
                            <div class="m-row ">
                                <div class="input-wrapper">
                                    <v-drop-down label="Đơn vị" tabindex="3" ref="cb" hasLabel hasInput :heightCb="47"
                                        v-model="dataForm.departmentName" :valueField="DepartmentName"
                                        rightIcon="combobox__btn" displayField="DepartmentName"
                                        :dataAll="DataDepartment.value" placeholder="Chọn đơn vị"
                                        @item-click="clickDataDepartment">
                                    </v-drop-down>
                                    <span class="m-input-wrapper__error"></span>
                                </div>
                            </div>
                            <div class="m-row">
                                <div class="input-wrapper">
                                    <v-input label="Chức danh" tabindex="4" hasLabel :maxLength="15"
                                        v-model="dataForm.positionName" :radius="false" placeholder="Chức danh"
                                        @blur="changeValueInput" @focus="changeValueInput"></v-input>
                                </div>
                            </div>
                        </div>
                        <div class="employee__right">
                            <div class="m-row display-flex">
                                <div class="input-wrapper width-40">
                                    <v-input-date label="Ngày sinh" hasLabel tabindex="5"
                                        v-model="dataForm.dateOfBirth">
                                    </v-input-date>
                                </div>
                                <div class="input-wrapper flex-1 margin-left-10px">
                                    <label class="m-input__label" for="">Giới tính</label>
                                    <div class="form__radio display-flex">
                                        <input class="radio" type="radio" id="rdMale" tabindex="6" name="gender"
                                            value="nam" /><span class="radio__title">Nam</span>
                                        <input class="radio" type="radio" id="rdFemale" name="gender" tabindex="7"
                                            value="nu" /><span class="radio__title">Nữ</span>
                                        <input class="radio" type="radio" id="rdOther" name="gender" tabindex="8"
                                            value="khac" /><span class="radio__title">Khác</span>
                                    </div>
                                </div>
                            </div>
                            <div class="m-row display-flex">
                                <div class="input-wrapper">
                                    <v-input label="Số CMND" tabindex="9" hasLabel :maxLength="15"
                                        v-model="dataForm.identityNumber" :radius="false" placeholder="CMND"
                                        @blur="changeValueInput" @focus="changeValueInput"></v-input>
                                </div>
                                <div class="input-wrapper margin-left-10px">
                                    <v-input-date label="Ngày cấp" hasLabel tabindex="10"
                                        v-model="dataForm.identityDate">
                                    </v-input-date>
                                </div>
                            </div>
                            <div class="m-row">
                                <div class="input-wrapper">
                                    <v-input label="Nơi cấp" tabindex="11" hasLabel :maxLength="15"
                                        v-model="dataForm.identityPlace" :radius="false" placeholder="Nơi cấp"
                                        @blur="changeValueInput" @focus="changeValueInput"></v-input>
                                </div>
                            </div>
                        </div>
                        <div class="employee__bottom">
                            <div class="m-row width-100">
                                <div class="input-wrapper">
                                    <v-input label="Địa chỉ" tabindex="12" hasLabel :maxLength="15"
                                        v-model="dataForm.address" :radius="false" placeholder=""
                                        @blur="changeValueInput" @focus="changeValueInput"></v-input>
                                </div>
                            </div>
                            <div class="m-row display-flex">
                                <div class="input-wrapper">
                                    <v-input label="ĐT di động" tabindex="13" hasLabel :maxLength="15"
                                        v-model="dataForm.phoneNumber" :radius="false" placeholder=""
                                        @blur="changeValueInput" @focus="changeValueInput"></v-input>
                                </div>
                                <div class="input-wrapper margin-left-10px">
                                    <v-input label="ĐT cố định" tabindex="14" hasLabel :maxLength="15"
                                        v-model="dataForm.phone" :radius="false" placeholder="" @blur="changeValueInput"
                                        @focus="changeValueInput"></v-input>
                                </div>
                                <div class="input-wrapper margin-left-10px">
                                    <v-input label="Email" tabindex="15" hasLabel :maxLength="15"
                                        v-model="dataForm.email" :radius="false" placeholder="" @blur="changeValueInput"
                                        @focus="changeValueInput"></v-input>
                                </div>
                            </div>
                            <div class="m-row display-flex">
                                <div class="input-wrapper">
                                    <v-input label="Tài khoản ngân hàng" tabindex="16" hasLabel :maxLength="15"
                                        v-model="dataForm.bankAccount" :radius="false" placeholder=""
                                        @blur="changeValueInput" @focus="changeValueInput"></v-input>
                                </div>
                                <div class="input-wrapper margin-left-10px">
                                    <v-input label="Tên ngân hàng" tabindex="17" hasLabel :maxLength="15"
                                        v-model="dataForm.bankName" :radius="false" placeholder=""
                                        @blur="changeValueInput" @focus="changeValueInput"></v-input>
                                </div>
                                <div class="input-wrapper margin-left-10px">
                                    <v-input label="Chi nhánh" tabindex="18" hasLabel :maxLength="15"
                                        v-model="dataForm.bankBranchName" :radius="false" placeholder=""
                                        @blur="changeValueInput" @focus="changeValueInput"></v-input>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="footer-popup">
                    <div class="employee__footer">
                        <v-tooltip content="Huỷ" placement="top" right="top">
                            <v-button text="Huỷ" tabindex="19" type="secodary" @click="handlePopupClose" radius>
                            </v-button>
                        </v-tooltip>
                        <div class="right__button">
                            <v-tooltip content="Cất" placement="top" right="top">
                                <v-button text="Cất" tabindex="20" type="secodary" @click="saveData" radius>
                                </v-button>
                            </v-tooltip>
                            <v-tooltip content="Cất và thêm" placement="top" right="top">
                                <v-button text="Cất và thêm" @click="saveData" tabindex="21" radius></v-button>
                            </v-tooltip>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </teleport>

    <!-- Toast message thêm mới thành công -->
    <v-message v-if="isShowMessage" textMessage="Thêm mới dữ liệu thành công" iconMessage="ic-success"></v-message>

    <!-- Dialog messagebox hủy bỏ khai báo -->
    <teleport to="body">
        <v-message-box leftIcon="ic-warning" :textMessageBox="Resource.TitleDialogMessage.AddAsset.VI"
            :disabledValueLeft="false" :disabledValueRight="false" v-if="isDialogMessCancelAdd">
            <v-button :text="Resource.TitleBtnDialog.Cancel.VI" radius></v-button>
            <v-button :text="Resource.TitleBtnDialog.NoCancel.VI" type="secodary" @click="isDialogMessCancelAdd = false"
                radius></v-button>
        </v-message-box>
    </teleport>

    <!-- Dialog messagebox cập nhật -->
    <teleport to="body">
        <v-message-box leftIcon="ic-warning" :textMessageBox="Resource.TitleDialogMessage.SaveUpdate.VI"
            :disabledValueLeft="false" :disabledValueRight="false" v-if="isDialogMessUpdate">
            <v-button :text="Resource.TitleBtnDialog.Save.VI" radius></v-button>
            <v-button :text="Resource.TitleBtnDialog.NoSave.VI" type="abort" radius></v-button>
            <v-button :text="Resource.TitleBtnDialog.Cancel.VI" type="secodary" radius></v-button>
        </v-message-box>
    </teleport>

    <teleport to="body">
        <v-message-box :disabledTop="true" title="Thông báo" icClose="ic-close" disabled disabledLeftMultiple
            :valueMultiple="titleErrValidate" :disabledMultiple="true" :disabledValueLeft="false"
            :disabledValueRight="false" v-if="isShowDialogDetail">
            <v-button :text="Resource.TitleBtnDialog.Close.VI" radius @click="handleCloseErrorMultiple"></v-button>
        </v-message-box>
    </teleport>
</template>
<script>
import {
    getCurrentInstance,
    onMounted,
    ref,
    watch,
    computed,
    resolveComponent as _resolveComponent,
    mergeProps as _mergeProps,
    onUpdated,
    nextTick,
} from "vue";
import {
    ssrRenderComponent as _ssrRenderComponent,
    ssrRenderAttrs as _ssrRenderAttrs,
} from "vue/server-renderer";
import VButton from "@/components/ms-control/ms-button/MsButton.vue";
import VInput from "@/components/ms-control/ms-text-box/MsTextBox.vue";
import useValidate from "@vuelidate/core";
import { required } from "@vuelidate/validators";
import VInputDate from "@/components/ms-control/ms-date-box/MsDateBox.vue";
import VDropDown from "@/components/ms-control/dropdown/MsDropdown.vue";
import VTooltip from "@/components/ms-control/tooltip/MsTooltip.vue";
import VMessageBox from "@/components/dialog/MsMessageBox.vue";
import Resource from "@/dictionary/resource";
import ResourceTable from "@/dictionary/resourceTable";
import Enum from "@/dictionary/enum.js";
import axios from "axios";

export default {
    name: "MsPopupEmployee",
    components: {
        VButton,
        VInput,
        VDropDown,
        VInputDate,
        VTooltip,
        VMessageBox,
    },
    props: {
        configStyle: {
            default: {},
        },
        statePopup: {
            default: false,
        },
        formModel: {
            default: {},
        },
        allData: {
            default: {},
        },
        dataPram: {
            default: {},
        },
    },
    methods: {
        close() {
            this.$parent.close();
        },
    },
    setup(props, { emit }) {
        const { proxy } = getCurrentInstance();
        //Show toastMessage
        window.popup = proxy;
        const isShowMessage = ref(false);

        const isShowPopup = ref(false);
        const isDialogMessCancelAdd = ref(false);

        const errorMessage = ref({});
        //Show dialog cập nhật
        const isDialogMessUpdate = ref(false);

        const titleErrValidate = ref([]);

        const isShowDialogDetail = ref(false);
        const isSubmited = ref(false);
        const dataForm = ref({
            Mode: 0,
            employeeId: "",
            employeeCode: "",
            employeeName: "",
            dateOfBirth: "",
            positionId: "",
            positionCode: "",
            positionName: "",
            departmentId: "",
            departmentCode: "",
            departmentName: "",
            identityNumber: "",
            identityDate: "",
            identityPlace: "",
            gender: 1,
            address: "",
            phoneNumber: "",
            phone: "",
            email: "",
            bankAccount: "",
            bankName: "",
            bankBranchName: "",
            modified_by: "",
            modified_date: "",
        });

        // Validate form
        const dataFormValidate = computed(() => {
            return {
                employeeCode: { required },
                employeeName: { required },
                departmentId: { required },
            };
        });
        const v$ = useValidate(dataFormValidate, dataForm);
        const DataDepartment = ref([]);

        const title = ref("");


        /**
         * Lấy dữ liệu toàn bộ đơn vị
         *  @author DuongNhung
         */
        async function loadDataDepartment() {
            try {
                await axios
                    .get('https://amis.manhnv.net/api/v1/Departments')
                    .then(response => {
                        let data = response.data || []; //?.Data;
                        proxy.DataDepartment.value = data;
                    })
            } catch (error) {
                console.log(error);
            }
        }

        //Sự kiện close error message Multiple
        const handleCloseErrorMultiple = () => {
            proxy.isShowDialogDetail = false;
            proxy.isSubmited = true;
            proxy.focusInput();
        };
        watch(
            () => dataForm.value,
            (newVal, old) => { },
        );
        onMounted(() => {
            proxy.focusInput();
        });

        const focusInput = () => {
            proxy.$refs.inputEmployeeCode.$el.getElementsByTagName("input")[0].focus();
        };


        onMounted(() => {
            try {
                /**
                 * Kiểm tra giá trị mode là add hay cập nhật
                 *  @author DuongNhung
                 */
                switch (proxy.formModel.mode) {
                    //Kiểm tra giá trị mode là cập nhật
                    case Enum.Mode.Update:
                        proxy.title = Resource.TitleFormPopup.FormUpdateEmployee.VI;
                        // Lấy dữ liệu nhân viên theo id nhân viên
                        proxy.dataForm = proxy.allData[0];
                        proxy.dataForm.Mode = 2;
                        break;

                    //Kiểm tra giá trị mode là thêm
                    case Enum.Mode.Add:
                        proxy.title = Resource.TitleFormPopup.FormAddEmployee.VI;
                        // proxy.getAssetNextCode();
                        proxy.dataForm.Mode = 1;
                        break;

                }
            } catch (error) {
                console.log(error);
            }
        });
        onMounted(() => {
            proxy.loadDataDepartment();
        });

        /**
         * Xét style cho dropdown
         *  @author DuongNhung
         */
        const styles = computed(() => {
            let arr = [];
            if (props.configStyle.width) {
                arr.push("width: " + props.configStyle.width + "px;");
                arr.push("min-width: " + props.configStyle.width + "px;");
            }
            if (props.configStyle.minWidth) {
                arr.push("min-width: " + props.configStyle.minWidth + "px;");
            }
            if (props.configStyle.height) {
                arr.push("height: " + props.configStyle.height + "px;");
                loadDataDepartment;
                arr.push("min-height: " + props.configStyle.height + "px;");
            }
            return arr.join("; ");
        });
        /**
         * Xử lý sự kiện click mã bộ phận câp nhật lại tên bộ phận cho input
         *  @author DuongNhung
         */
        const clickDataDepartment = (item) => {
            proxy.dataForm.departmentName = item.departmentName;
        };
        const onBlurInput = (isValue, valueField, e) => {
            switch (valueField) {
                case "employeeName": {
                    if (isValue != "") {
                        proxy.errorMessage.employeeName = false;
                        proxy.dataForm.department_name = isValue;
                    } else {
                        proxy.errorMessage.employeeName = true;
                    }
                    break;
                }
                case "employeeCode": {
                    if (isValue != "") {
                        proxy.errorMessage.employeeCode = false;
                        proxy.dataForm.employeeCode = isValue;
                    } else {
                        proxy.errorMessage.employeeCode = true;
                    }

                    break;
                }

                default: {
                    break;
                }
            }
        };

        /**
         * Xử lý cập nhật lại dữ liệu dataForm
         *  @pram {isValue,valueField} - valueField: Trường dữ liệu, isValue: giá trị
         *  @author DuongNhung
         */
        const changeValueInput = (isValue, valueField) => {
            switch (valueField) {
                case "departmentName": {
                    proxy.dataForm.departmentName = isValue;
                    break;
                }
                case "dateOfBirth": {
                    proxy.dataForm.dateOfBirth = isValue;
                    break;
                }
                case "positionName": {
                    proxy.dataForm.positionName = isValue;
                    break;
                }
                case "identityNumber": {
                    proxy.dataForm.identityNumber = isValue;
                    break;
                }
                case "identityDate": {
                    proxy.dataForm.identityDate = isValue;
                    break;
                }
                case "identityPlace": {
                    proxy.dataForm.identityPlace = isValue;
                    break;
                }
                case "address": {
                    proxy.dataForm.address = isValue;
                    break;
                }
                case "phoneNumber": {
                    proxy.dataForm.phoneNumber = isValue;
                    break;
                }
                case "phone": {
                    proxy.dataForm.phone = isValue;
                    break;
                }
                case "email": {
                    proxy.dataForm.email = isValue;
                    break;
                }
                case "bankAccount": {
                    proxy.dataForm.bankAccount = isValue;
                    break;
                }
                case "bankName": {
                    proxy.dataForm.bankName = isValue;
                    break;
                }
                case "bankBranchName": {
                    proxy.dataForm.bankBranchName = isValue;
                    break;
                }
                default: {
                    break;
                }
            }
        };
        // Validate dữ liệu
        const validateData = () => {
            proxy.v$.$validate();
            if (proxy.v$.$error) {
                proxy.titleErrValidate = [];
                proxy.errorMessage = {};
                if (proxy.dataForm.employeeCode == "") {
                    proxy.titleErrValidate.push(Resource.ErrorValidate.employeeCode.VI);
                    proxy.errorMessage.employeeCode = true;
                }
                if (proxy.dataForm.employeeName == "") {
                    proxy.titleErrValidate.push(Resource.ErrorValidate.employeeName.VI);
                    proxy.errorMessage.employeeName = true;
                }

                if (proxy.dataForm.departmentName == "") {
                    proxy.titleErrValidate.push(Resource.ErrorValidate.departmentName.VI);
                    proxy.errorMessage.departmentName = true;
                }
            }
            // else 
            // if (proxy.dataForm.depreciation_year > proxy.dataForm.cost) {
            //     proxy.titleErrValidate = [];
            //     proxy.errorMessage = {};
            //     proxy.titleErrValidate.push(
            //         "Hao mòn năm phải nhỏ hơn hoặc bằng nguyên giá"
            //     );
        };

        const saveData = () => {
            try {
                if (proxy.validateData() == false) {
                    proxy.isShowDialogDetail = true;
                } else {
                    switch (proxy.formModel.mode) {
                        //Kiểm tra giá trị mode là cập nhật
                        case Enum.Mode.Update: {
                            proxy.isDialogMessUpdate = true;

                            break;
                        }
                        //Kiểm tra giá trị mode là thêm hay nhân bản
                        case (Enum.Mode.Add, Enum.Mode.Duplicate): {
                            proxy.isShowMessage = true;
                            break;
                        }

                        default:
                            break;
                    }
                }
            } catch (error) {
                console.error(error);
            }
        };
        const handlePopupClose = () => {
            let closes = proxy.validateData();
            if (closes == true) {
                proxy.isDialogMessCancelAdd = true;
            } else {
                emit("closePopup", proxy.isShowPopup);
            }

            // proxy.isShowPopup = false;
        };

        return {
            styles,
            title,
            isShowPopup,
            isShowMessage,
            titleErrValidate,
            isDialogMessCancelAdd,
            isDialogMessUpdate,
            Resource,
            ResourceTable,
            dataForm,
            DataDepartment,
            isShowDialogDetail,
            loadDataDepartment,
            clickDataDepartment,
            focusInput,
            changeValueInput,
            // getAssetNextCode,
            saveData,
            handlePopupClose,
            errorMessage,
            validateData,
            onBlurInput,
            dataFormValidate,
            handleCloseErrorMultiple,
            isSubmited,
            v$,
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
@import "@/style/layout/EmployeePopup.scss";
@import "@/style/components/MsInputDate.scss";
</style>
  