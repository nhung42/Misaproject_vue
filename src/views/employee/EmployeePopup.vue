<!-- eslint-disable vue/html-indent -->
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
                                    tabindex="23" ref="lastIndex">
                                <label for="supplier"> Là nhà cung cấp</label><br>
                            </div>
                        </div>
                    </div>
                    <button class="dialog__button display-flex">
                        <v-tooltip content="Giúp(F1)" placement="bottom" right="bottom">
                            <div class="icon-help"></div>
                        </v-tooltip>
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
                                    <v-input label="Mã" tabindex="1" hasLabel hasInput valueField="EmployeeCode"
                                        ref="inputemployeeCode" v-model="dataForm.EmployeeCode" :radius="false"
                                        placeholder="Mã nhân viên" @blur="onBlurInput" @focus="changeValueInput"
                                        :disabledMessage="errorMessage.employeeCode"
                                        :message="Resource.ErrorInput.EmployeeCode">
                                    </v-input>
                                </div>
                                <div class="input-wrapper margin-left-10px flex-1">
                                    <v-input label="Họ và tên" tabindex="2" hasLabel hasInput valueField="EmployeeName"
                                        v-model="dataForm.EmployeeName" :radius="false" placeholder="Tên nhân viên"
                                        @blur="onBlurInput" @focus="changeValueInput"
                                        :disabledMessage="errorMessage.employeeName"
                                        :message="Resource.ErrorInput.EmployeeName"></v-input>
                                </div>
                            </div>
                            <div class="m-row ">
                                <div class="input-wrapper">
                                    <v-drop-down label="Đơn vị" tabindex="3" ref="cb" hasLabel hasInput :heightCb="40"
                                        v-model="dataForm.DepartmentName" valueField="DepartmentName"
                                        rightIcon="combobox__btn" displayField="DepartmentName"
                                        :dataAll="DataDepartment.value" placeholder="Chọn đơn vị"
                                        @item-click="clickDataDepartment" :disabledMessage="errorMessage.departmentName"
                                        :message="Resource.ErrorInput.DepartmentName" @focusout="onBlurDropdown"
                                        @focus="changeValueInput">
                                    </v-drop-down>
                                    <span class="m-input-wrapper__error"></span>
                                </div>
                            </div>
                            <div class="m-row">
                                <div class="input-wrapper">
                                    <v-input label="Chức danh" tabindex="4" hasLabel valueField="PositionName"
                                        v-model="dataForm.PositionName" :radius="false" placeholder="Chức danh"
                                        @blur="onBlurInput" @focus="changeValueInput"></v-input>
                                </div>
                            </div>
                        </div>
                        <div class="employee__right">
                            <div class="m-row display-flex">
                                <div class="input-wrapper width-40">
                                    <v-input-date label="Ngày sinh" hasLabel tabindex="5" valueField="DateOfBirth"
                                        v-model="dataForm.DateOfBirth" placeholder="dd/mm/yy">
                                    </v-input-date>
                                </div>
                                <div class="input-wrapper flex-1 margin-left-10px">
                                    <label class="m-input__label" for="">Giới tính</label>
                                    <div class="form__radio display-flex">
                                        <div tabindex="6">
                                            <input class="radio" type="radio" id="rdMale" name="gender" value="1"
                                                v-model="dataForm.Gender" :checked="dataForm.Gender === 1" />
                                        </div><span class="radio__title">Nam</span>
                                        <div tabindex="7">
                                            <input class="radio" type="radio" id="rdFemale" name="gender" value="0"
                                                v-model="dataForm.Gender" :checked="dataForm.Gender === 0" />
                                        </div><span class="radio__title">Nữ</span>

                                        <div tabindex="8">
                                            <input class="radio" type="radio" id="rdOther" name="gender" value="2"
                                                v-model="dataForm.Gender" :checked="dataForm.Gender === 2" />
                                        </div><span class="radio__title">Khác</span>

                                    </div>
                                </div>
                            </div>
                            <div class="m-row display-flex">
                                <div class="input-wrapper">
                                    <v-input label="Số CMND" tabindex="9" hasLabel valueField="IdentityNumber"
                                        title="Số Chứng Minh Nhân Dân" v-model="dataForm.IdentityNumber" :radius="false"
                                        placeholder="CMND" @blur="onBlurInput" @focus="changeValueInput"></v-input>
                                </div>
                                <div class="input-wrapper margin-left-10px">
                                    <v-input-date label="Ngày cấp" hasLabel tabindex="10" valueField="IdentityDate"
                                        v-model="dataForm.IdentityDate">
                                    </v-input-date>
                                </div>
                            </div>
                            <div class="m-row">
                                <div class="input-wrapper">
                                    <v-input label="Nơi cấp" tabindex="11" hasLabel valueField="IdentityPlace"
                                        v-model="dataForm.IdentityPlace" :radius="false" placeholder="Nơi cấp"
                                        @blur="onBlurInput" @focus="changeValueInput"></v-input>
                                </div>
                            </div>
                        </div>
                        <div class="employee__bottom">
                            <div class="m-row width-100">
                                <div class="input-wrapper">
                                    <v-input label="Địa chỉ" tabindex="12" hasLabel valueField="Address"
                                        v-model="dataForm.Address" :radius="false" placeholder="" @blur="onBlurInput"
                                        @focus="changeValueInput"></v-input>
                                </div>
                            </div>
                            <div class="m-row display-flex">
                                <div class="input-wrapper">
                                    <v-input label="ĐT di động" tabindex="13" hasLabel valueField="PhoneNumber"
                                        title="Số Điện Thoại Di Động" v-model="dataForm.PhoneNumber" :radius="false"
                                        placeholder="" @blur="onBlurInput" @focus="changeValueInput"></v-input>
                                </div>
                                <div class="input-wrapper margin-left-10px">
                                    <v-input label="ĐT cố định" tabindex="14" hasLabel valueField="Phone"
                                        title="Số Điện Thoại Cố Định" v-model="dataForm.Phone" :radius="false"
                                        placeholder="" @blur="onBlurInput" @focus="onBlurInput"></v-input>
                                </div>
                                <div class="input-wrapper margin-left-10px">
                                    <v-input label="Email" tabindex="15" hasLabel valueField="Email"
                                        v-model="dataForm.Email" :radius="false" placeholder="" @blur="onBlurInput"
                                        @focus="changeValueInput"></v-input>
                                </div>
                            </div>
                            <div class="m-row display-flex">
                                <div class="input-wrapper">
                                    <v-input label="Tài khoản ngân hàng" tabindex="16" hasLabel valueField="BankAccount"
                                        v-model="dataForm.BankAccount" :radius="false" placeholder=""
                                        @blur="onBlurInput" @focus="changeValueInput"></v-input>
                                </div>
                                <div class="input-wrapper margin-left-10px">
                                    <v-input label="Tên ngân hàng" tabindex="17" hasLabel valueField="BankName"
                                        v-model="dataForm.BankName" :radius="false" placeholder="" @blur="onBlurInput"
                                        @focus="changeValueInput"></v-input>
                                </div>
                                <div class="input-wrapper margin-left-10px">
                                    <v-input label="Chi nhánh" tabindex="18" hasLabel valueField="BankBranchName"
                                        v-model="dataForm.BankBranchName" :radius="false" placeholder=""
                                        @blur="onBlurInput" @focus="changeValueInput"></v-input>
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
                                <v-button text="Cất" tabindex="20" type="secodary" @click="saveData(true)" radius>
                                </v-button>
                            </v-tooltip>
                            <v-tooltip content="Cất và thêm" placement="top" right="top">
                                <v-button text="Cất và thêm" @click="saveData(false)" tabindex="21" radius></v-button>
                            </v-tooltip>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </teleport>
    <!-- Dialog messagebox hủy bỏ khai báo -->
    <teleport to="body">
        <ms-message-box leftIcon="icon-warning" title="Thông báo"
            :textMessageBox="Resource.TitleDialogMessage.AddAsset.VI" :disabledValueLeft="false"
            :disabledValueRight="false" v-if="isDialogMessCancelAdd">
            <v-button :text="Resource.TitleBtnDialog.Cancel.VI" radius></v-button>
            <v-button :text="Resource.TitleBtnDialog.NoCancel.VI" type="secodary" @click="isDialogMessCancelAdd = false"
                radius></v-button>
        </ms-message-box>
    </teleport>

    <!-- Dialog messagebox cập nhật -->
    <teleport to="body">
        <ms-message-box leftIcon="icon-warring" title="Thông báo"
            :textMessageBox="Resource.TitleDialogMessage.SaveUpdate.VI" :disabledValueLeft="false"
            :disabledValueRight="false" v-if="isDialogMessUpdate">
            <v-button :text="Resource.TitleBtnDialog.Save.VI" radius @click="updateData(true)"></v-button>
            <v-button :text="Resource.TitleBtnDialog.Cancel.VI" type="secodary" radius></v-button>
        </ms-message-box>
    </teleport>

    <teleport to="body">
        <ms-message-box :disabledTop="true" title="Thông báo" icClose="ic-close" disabled disabledLeftMultiple
            :valueMultiple="titleErrValidate" :disabledMultiple="true" :disabledValueLeft="false"
            :disabledValueRight="false" v-if="isShowDialogDetail">
            <v-button :text="Resource.TitleBtnDialog.Close.VI" radius @click="handleCloseErrorMultiple"></v-button>
        </ms-message-box>
    </teleport>
    <!-- Dialog thông báo lỗi khi call api-->
    <teleport to="body">
        <ms-message-box leftIcon="icon-error" :textMessageBox="error" :disabledValueLeft="false"
            :disabledValueRight="false" v-if="isDialogError">
            <v-button :text="Resource.TitleBtnDialog.Close.VI" @click="isDialogError = false" radius></v-button>
        </ms-message-box>
    </teleport>
    <!-- Toast message thêm mới thành công -->
    <teleport to='body'>
        <ms-message v-if="isShowMessage" toastAct=" Thêm" @closeOpenToast="closeOpenToast">
        </ms-message>
    </teleport>
    <!-- Toast message update thành công -->
    <teleport to='body'>
        <ms-message v-if="isShowMessageUpdate" toastAct=" Sửa" @closeOpenToast="closeOpenToast">
        </ms-message>
    </teleport>
</template>
<script>
import {
    getCurrentInstance,
    onMounted,
    ref,
    watchEffect,
    computed,
} from "vue";
import {
} from "vue/server-renderer";
import VButton from "@/components/ms-control/ms-button/MsButton.vue";
import VInput from "@/components/ms-control/ms-text-box/MsTextBox.vue";
import useVuelidate from "@vuelidate/core";
import { required } from "@vuelidate/validators";
import VInputDate from "@/components/ms-control/ms-date-box/MsDateBox.vue";
import VDropDown from "@/components/ms-control/dropdown/MsDropdown.vue";
import VTooltip from "@/components/ms-control/tooltip/MsTooltip.vue";
import MsMessageBox from "@/components/dialog/MsMessageBox.vue";
import Resource from "@/dictionary/resource";
import ResourceTable from "@/dictionary/resourceTable";
import Enum from "@/dictionary/enum.js";
import axios from "axios";
import MsMessage from "@/components/dialog/MSToastMessage.vue";

export default {
    name: "MsPopupEmployee",
    components: {
        VButton,
        VInput,
        VDropDown,
        VInputDate,
        VTooltip,
        MsMessageBox,
        MsMessage,
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
        /**
         * Hàm đóng toast message khi cất hoặc thêm
         * author Duong Nhung
         */
        closeOpenToast() {

            if (this.closeForm === true) {
                this.isShowMessage = !this.isShowMessage
                console.log(this.closeForm);
                this.$emit('closePopup');
            }
            else {
                this.isShowMessage = !this.isShowMessage;
                this.dataForm = { ...this.dataFormNull };
            }
        }
    },
    emits: ["closePopup"],
    setup(props, { emit }) {
        const { proxy } = getCurrentInstance();
        window.popup = proxy;
        //Show message khi thêm mới thành công
        const isShowMessage = ref(false);
        //Show popup
        const isShowPopup = ref(false);
        //Message cancel Add
        const isDialogMessCancelAdd = ref(false);
        //Message update thành công 
        const isShowMessageUpdate = ref(false);
        //Dialog message lỗi
        const isDialogError = ref(false);
        //Message lỗi khi validate
        const errorMessage = ref({});
        //Show dialog cập nhật
        const isDialogMessUpdate = ref(false);
        //Nội dung lỗi khi validate
        const titleErrValidate = ref([]);
        //Dialog hiển thị lỗi
        const isShowDialogDetail = ref(false);
        //State submit
        const isSubmited = ref(false);
        //Dữ liệu form
        const closeForm = ref(false);
        const dataForm = ref({
            Mode: 0,
            EmployeeCode: "",
            EmployeeName: "",
            DateOfBirth: "",
            PositionCode: "",
            PositionName: "",
            DepartmentId: "",
            DepartmentCode: "",
            DepartmentName: "",
            IdentityNumber: "",
            IdentityDate: "",
            IdentityPlace: "",
            Gender: 1,
            GenderName: "",
            Address: "",
            PhoneNumber: "",
            phone: "",
            Email: "",
            BankAccount: "",
            BankName: "",
            BankBranchName: "",
            Modified_by: "",
            Modified_date: "",
        });
        const dataFormNull = ref({
            Mode: 0,
            EmployeeCode: "",
            EmployeeName: "",
            DateOfBirth: "",
            PositionCode: "",
            PositionName: "",
            DepartmentId: "",
            DepartmentCode: "",
            DepartmentName: "",
            IdentityNumber: "",
            IdentityDate: "",
            IdentityPlace: "",
            Gender: 1,
            GenderName: "",
            Address: "",
            PhoneNumber: "",
            phone: "",
            Email: "",
            BankAccount: "",
            BankName: "",
            BankBranchName: "",
            Modified_by: "",
            Modified_date: "",
        });
        // Validate form
        const dataFormValidate = computed(() => {
            return {
                EmployeeCode: { required },
                EmployeeName: { required },
                DepartmentId: { required },
            };
        });
        //Gọi hàm validate cho dataForm
        const v$ = useVuelidate(dataFormValidate, dataForm);
        //Dữ liệu của đơn vị
        const DataDepartment = ref([]);
        //title popup
        const title = ref("");
        //Lỗi
        const error = ref("");

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
                    .catch(function (error) {
                        proxy.error = error.response.data.devMsg;
                        proxy.isDialogError = true;

                    });
            } catch (error) {
                console.log(error);
            }
        }
        /**
         * Call api thêm mới thông tin employee
         * @param {*} obj 
         * author DuongNhung
         */
        async function addEmployee(obj) {
            try {
                axios
                    .post("https://amis.manhnv.net/api/v1/Employees", obj)
                    .then((res) => {
                        console.log("post:", res.data);
                        proxy.isShowMessage = true;
                    })
                    .catch(function (error) {
                        proxy.error = error.response.data.devMsg;
                        proxy.isDialogError = true;

                    });
            }
            catch (error) {
                console.log(error);
            }
        }
        /**
         * Call api update thông tin nhân viên
         * @param {*} obj 
         * author DuongNhung
         */
        async function updateEmployee(obj) {
            try {
                axios
                    .put("https://amis.manhnv.net/api/v1/Employees/" + obj.EmployeeId, obj)
                    .then((res) => {
                        console.log("post:", res.data);
                        proxy.isShowMessageUpdate = true;
                    })
                    .catch(function (error) {
                        proxy.error = error.response.data.devMsg;
                        proxy.isDialogError = true;

                    });
            }
            catch (error) {
                console.log(error);
            }
        }
        /**
         * Hàm lấy giá trị chon radio gender
         * @author DuongNhung
         */
        const getGender = () => {
            if (proxy.dataForm.Gender === 1) {
                proxy.dataForm.GenderName = "Nam";
            }
            if (proxy.dataForm.GenderName === 0) {
                proxy.dataForm.GenderName = "Nữ"
            }
            else {
                proxy.dataForm.GenderName = "Khác";
            }
        }
        /**
         * Sửa thông thin nhân viên,load lại data
         * @author DuongNhung
         */
        const updateData = (close) => {
            if (proxy.validateData() == false) {
                proxy.isShowDialogDetail = true;
            }
            else {
                proxy.closeForm = close;
                proxy.isDialogMessUpdate = false;
                proxy.updateEmployee(dataForm.value);
                proxy.isShowPopup = false;
            }
        }
        //Sự kiện close error message Multiple Error
        const handleCloseErrorMultiple = () => {
            proxy.isShowDialogDetail = false;
            proxy.isSubmited = true;
            proxy.focusInput();
        };
        watchEffect(
            () => dataForm.value,
        );
        //Focus ô input đầu tiên
        onMounted(() => {
            proxy.focusInput();
        });
        /**
         * Focus ô input đầu tiên của form
         * @author DuongNhung
         */
        const focusInput = () => {
            proxy.$refs.inputemployeeCode.$el.getElementsByTagName("input")[0].focus();
        };
        onMounted(() => {
            try {
                /**
                 * Kiểm tra giá trị mode là add hay cập nhật hoặc nhân bản
                 *  @author DuongNhung
                 */
                switch (proxy.formModel.mode) {
                    //Kiểm tra giá trị mode là cập nhật
                    case Enum.Mode.Update:
                        proxy.title = Resource.TitleFormPopup.FormUpdateEmployee.VI;
                        // Lấy dữ liệu nhân viên theo id nhân viên
                        proxy.dataForm = proxy.dataPram;
                        proxy.dataForm.Mode = 2;
                        break;

                    //Kiểm tra giá trị mode là thêm
                    case Enum.Mode.Add:
                        proxy.title = Resource.TitleFormPopup.FormAddEmployee.VI;
                        proxy.dataForm.Mode = 1;
                        break;

                    //Kiểm tra giá trị mode là nhân bản
                    case Enum.Mode.Duplicate:
                        proxy.title = Resource.TitleFormPopup.FormAddEmployee.VI;
                        // Lấy dữ liệu nhân viên theo id nhân viên
                        proxy.dataForm = proxy.dataPram;
                        proxy.dataForm.Mode = 3;
                        break;

                }
            } catch (error) {
                console.log(error);
            }
        });
        onMounted(() => {
            //Gọi hàm lấy data đơn vị
            proxy.loadDataDepartment();
            //Gọi hmaf lấy giá trị gender
            proxy.getGender();
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
         * Xử lý sự kiện click nhận tên đơn vị cho for
         *  @author DuongNhung
         */
        const clickDataDepartment = (item) => {
            proxy.dataForm.DepartmentId = item.DepartmentId;
            proxy.dataForm.DepartmentCode = item.DepartmentCode;
            proxy.dataForm.DepartmentName = item.DepartmentName;
            proxy.errorMessage.departmentName = false;
        };
        /**
         * Hàm gán giá trị của input dropdown khi blur ra khỏi ô input
         * @param {*} disp 
         * @param {*} valueField 
         * @author DuongNhung
         */
        const onBlurDropdown = (disp, valueField) => {

            switch (valueField) {
                case "DepartmentName":

                    if (disp === null) {
                        proxy.errorMessage.departmentName = true;
                    } else {
                        proxy.errorMessage.departmentName = false;
                        proxy.dataForm.DepartmentName = disp;
                    }
                    break;
            }


        }
        /**
         * Hàm gán giá trị của input khi blur ra khỏi ô input
         * @param {*} disp 
         * @param {*} valueField 
         * @author DuongNhung
         */
        const onBlurInput = (isValue, valueField) => {
            switch (valueField) {

                case "EmployeeName": {
                    if (isValue != "") {
                        proxy.errorMessage.employeeName = false;
                        proxy.dataForm.EmployeeName = isValue;
                    } else {
                        proxy.errorMessage.employeeName = true;
                    }
                    break;
                }
                case "EmployeeCode": {
                    if (isValue != "") {
                        proxy.errorMessage.employeeCode = false;
                        proxy.dataForm.EmployeeCode = isValue;
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
                case "EmployeeCode": {
                    proxy.dataForm.EmployeeCode = isValue;
                    break;
                }
                case "EmployeeName": {
                    proxy.dataForm.EmployeeName = isValue;
                    break;
                }
                case "DepartmentName": {
                    proxy.dataForm.DepartmentName = isValue;
                    break;
                }
                case "DateOfBirth": {
                    proxy.dataForm.DateOfBirth = isValue;
                    break;
                }
                case "PositionName": {
                    proxy.dataForm.PositionName = isValue;
                    break;
                }
                case "IdentityNumber": {
                    proxy.dataForm.IdentityNumber = isValue;
                    break;
                }
                case "IdentityDate": {
                    proxy.dataForm.IdentityDate = isValue;
                    break;
                }
                case "IdentityPlace": {
                    proxy.dataForm.IdentityPlace = isValue;
                    break;
                }
                case "Address": {
                    proxy.dataForm.Address = isValue;
                    break;
                }
                case "PhoneNumber": {
                    proxy.dataForm.PhoneNumber = isValue;
                    break;
                }
                case "Phone": {
                    proxy.dataForm.Phone = isValue;
                    break;
                }
                case "Email": {
                    proxy.dataForm.Email = isValue;
                    break;
                }
                case "BankAccount": {
                    proxy.dataForm.BankAccount = isValue;
                    break;
                }
                case "BankName": {
                    proxy.dataForm.BankName = isValue;
                    break;
                }
                case "BankBranchName": {
                    proxy.dataForm.BankBranchName = isValue;
                    break;
                }
                default: {
                    break;
                }
            }
        };
        /**
         * Hàm validate dữ liệu
         * @author DuongNhung
        */
        const validateData = () => {
            proxy.v$.$validate();
            if (proxy.v$.$error) {
                proxy.titleErrValidate = [];
                proxy.errorMessage = {};
                if (proxy.dataForm.EmployeeCode == "") {
                    proxy.titleErrValidate.push(Resource.ErrorValidate.employeeCode);
                    proxy.errorMessage.employeeCode = true;
                }
                if (proxy.dataForm.EmployeeName == "") {
                    proxy.titleErrValidate.push(Resource.ErrorValidate.employeeName);
                    proxy.errorMessage.employeeName = true;
                }

                if (proxy.dataForm.DepartmentName == "") {
                    proxy.titleErrValidate.push(Resource.ErrorValidate.departmentName);
                    proxy.errorMessage.departmentName = true;
                }
            }
        };
        /**
         * Save Data
         * @author DuongNhung
         */
        const saveData = (close) => {
            try {
                if (proxy.validateData() == false) {
                    proxy.isShowDialogDetail = true;
                } else {
                    proxy.closeForm = close;
                    switch (proxy.formModel.mode) {
                        //Kiểm tra giá trị mode là cập nhật
                        case Enum.Mode.Update: {
                            proxy.isDialogMessUpdate = true;
                            break;
                        }
                        //Kiểm tra giá trị mode 
                        case (Enum.Mode.Add): {
                            proxy.addEmployee(dataForm.value);
                            break;
                        }

                        //Kiểm tra giá trị mode là nhân bản 
                        case (Enum.Mode.Duplicate): {

                            proxy.addEmployee(dataForm.value);
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
        /**
         * Hàm đóng popup
         * @author DuongNhung
         */
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
            error,
            isShowPopup,
            isShowMessage,
            titleErrValidate,
            isDialogMessCancelAdd,
            isDialogMessUpdate,
            isShowMessageUpdate,
            Resource,
            ResourceTable,
            DataDepartment,
            isShowDialogDetail,
            isDialogError,
            closeForm,
            loadDataDepartment,
            clickDataDepartment,
            updateData,
            focusInput,
            dataForm,
            dataFormNull,
            getGender,
            changeValueInput,
            addEmployee,
            updateEmployee,
            saveData,
            handlePopupClose,
            errorMessage,
            validateData,
            onBlurDropdown,
            onBlurInput,
            dataFormValidate,
            handleCloseErrorMultiple,
            isSubmited,
            v$,
        };
    }
};
</script>
<style lang="scss" scoped>
input[type="radio"] {
    accent-color: #409330 !important;
    background-color: #fff;
}

.radio__title {
    font-family: Roboto;
    font-size: 14px;
}

@import "@/style/layout/EmployeePopup.scss";
@import "@/style/components/MsInputDate.scss";
@import "@/style/components/MsCheckbox.scss"
</style>
  