<template>
    <div class="filter-dropdown" ref="container" :class="disabledMessage ? 'mg-9' : false">
        <label class="text-label" v-if="hasLabel">
            {{ label ? label : "" }}
            <span v-if="hasInput">&#8727;</span>
        </label>
        <div class="combobox">
            <button class="dropdown-menu-toggle" ref="dropdown" :class="disabledMessage ? 'error__message' : ''">
                <input type="text" v-model="disp" :tabindex="tabindex" ref="input" :placeholder="placeholder"
                    v-on="eventListsioner" @keyup="search" @focus="isShowMenu = true" />
                <div :class="[
                    'app-icon',
                    rightIcon,
                    disabled ? 'disabled-icon' : '',
                ]" v-if="rightIcon" @click="isShowMenu = !isShowMenu"></div>
            </button>

            <teleport to="body">
                <div class="dropdown-menu" ref="drop" :style="style" :class="autoHeight ? 'height_auto--cbo' : ''"
                    v-if="isShowMenu">
                    <div class="dropdown-content">
                        <ul class="list-item--dropdown">
                            <dropdown-item v-for="item in data" :key="item" :dataItem="item"
                                :displayField="displayField" :class="[
                                    modelValue && modelValue == item[valueField] ? 'selected' : '',
                                ]" @menu-item-click="itemClick">
                            </dropdown-item>
                        </ul>
                    </div>
                </div>
            </teleport>
        </div>
        <span v-if="disabledMessage" class="error-message">{{
                message ? message : ""
        }}</span>
    </div>
</template>
<script>
import {
    getCurrentInstance,
    onMounted,
    ref,
    watch,
    computed,
    nextTick,
    reactive
} from "vue";
import DropdownItem from "./MsMenuItem.vue";
export default {
    name: "MsDropdown",
    components: {
        DropdownItem,
    },
    props: {
        //Id 
        id: {
            default: null,
            type: String,
        },
        modelValue: {
            default: null,
        },
        texts: {
            default: null,
            type: String,
        },
        //Icon tr??i
        leftIcon: {
            default: null,
            type: String,
        },
        //Data dropdown
        dataAll: {
            default: [],
        },
        //Icon ph???i
        rightIcon: {
            default: null,
            type: String,
        },
        //Kh??ng ???????c active
        disabled: {
            default: false,
            type: Boolean,
        },
        //placeholder
        placeholder: {
            default: null,
            type: [Number, String],
        },
        //Gi?? tr??? c???a dropdown
        valueField: {
            default: null,
            type: String,
        },
        //Gi?? tr??? hi???n th??? c???a dropdown
        displayField: {
            default: null,
            type: String,
        },
        //Label dropdown
        label: {
            default: null,
            type: String,
        },
        //c?? ???????c nh???p hay kh??ng
        hasInput: {
            default: false,
            type: Boolean,
        },
        //C?? label kh??ng
        hasLabel: {
            default: false,
            type: Boolean,
        },
        heightCb: {
            default: 0,
        },
        readonly: {
            default: false,
            type: Boolean,
        },
        tabindex: {
            default: null,
            type: String,
        },
        message: {
            default: null,
            type: String,
        },
        disabledMessage: {
            default: false,
            type: Boolean,
        },
    },
    methods: {
    },
    setup(props, { emit }) {
        const { proxy } = getCurrentInstance();
        //g??n data dropdown
        const data = ref(props.dataAll);
        const disp = ref("");
        const autoHeight = ref(false);
        const forcused = ref(false);

        window.cb = proxy;

        // L???y d??? li???u nh???ng item selected
        const selected = computed(() => {
            if (proxy.modelValue) {
                return proxy.dataAll.find(
                    (x) => x[proxy.valueField] == proxy.modelValue
                );
            } else {
                return null;
            }
        });
        onMounted(() => {
            proxy.data = proxy.dataAll;
            proxy.disp = display.value;
            watch(
                () => proxy.dataAll,
                () => {
                    disp.value = display.value;
                    proxy.data = proxy.dataAll;
                }
            );
        });

        /**
         * T??m ki???m d??? li???u trong dropdown
         * @param {} e 
         * @author DuongNhung
         */
        const search = function (e) {
            console.log(e);
            setTimeout(() => {
                let val = proxy.$refs.input.value;
                proxy.data = proxy.dataAll.filter((x) =>
                    x[props.displayField].toLowerCase()?.includes(val.toLowerCase())
                );
                // Ki???m tra gi?? tr??? input kh??c "" hay k
                if (val != "") {
                    proxy.autoHeight = true;
                }
                // disp.value = display.value;
            }, 100);
        };
        //Gi?? tr??? hi???n th??? l?? gi?? tr??? ???????c ch???n
        const display = computed(
            () => proxy.selected && proxy.selected[proxy.displayField]
        );
        //Style v??? tr?? cho dropdown
        const offsetPosi = reactive({
            top: 0,
            left: 0,
        });
        //Style v??? tr?? cho dropdown
        const offsetDropdown = reactive({
            width: 0,
            height: 0,
        });

        // Bi???n show menu list item dropdown
        const isShowMenu = ref(false);

        /**
         * Xet style cho dropdown
         * Author: DuongNhung
         */
        const style = computed(() => {
            let arr = [];
            arr.push(`top: ${offsetPosi.top}px`);
            arr.push(`left: ${offsetPosi.left}px`);
            arr.push(`width: ${offsetDropdown.width}px`);
            arr.push(`height: ${offsetDropdown.height}px`);
            return arr.join("; ");
        });

        /**
         * Theo d??i bi???n isShowMenu l?? true hay false
         * Author: DuongNhung
         */
        watch(
            () => isShowMenu.value,
            (newVal) => {
                if (newVal) {
                    proxy.setPosition();
                }
            }
        );

        /**
         * X??? ki???n click itemdropdown
         * Author: DuongNhung
         */
        const itemClick = (item) => {
            emit("update:modelValue", item[proxy.valueField]);
            nextTick(() => {
                emit("item-click", item);
            });
        };

        watch(
            () => proxy.modelValue,
            () => {
                disp.value = display.value;
            }
        );
        onMounted(() => {
            proxy.setPosition();
            proxy.setDropdown();
            proxy.initEvent();
        });

        /**
         * Set v??? tr?? cho dropdown
         * Author: DuongNhung
         */
        function setPosition() {
            let offset = proxy.$refs.dropdown.getBoundingClientRect();
            offsetPosi.top = offset.bottom;
            offsetPosi.left = offset.left;
        }

        //X??? l?? s??? ki???n
        const cancelEvent = (e) => {
            if (e) {
                //ph????ng th???c h???y s??? ki???n n???u n?? c?? th??? h???y ???????c, ngh??a l?? h??nh ?????ng m???c ?????nh c???a s??? ki???n s??? kh??ng x???y ra.
                if (typeof e.preventDefault === "function") {
                    e.preventDefault();
                }
                //ph????ng th???c ng??n kh??ng cho s??? lan truy???n c???a c??ng m???t s??? ki???n ???????c g???i.
                if (typeof e.stopPropagation === "function") {
                    e.stopPropagation();
                }
                //S??? ki???n ng??n kh??ng cho ng?????i nghe kh??c c???a c??ng m???t s??? ki???n ???????c g???i.
                if (typeof e.stopImmediatePropagation === "function") {
                    e.stopImmediatePropagation();
                }
            }
        };

        onMounted(() => {
            document.addEventListener("click", (e) => {
                let target = e.target;

                let cont = target.closest(".filter-dropdown");
                if (cont && cont.isEqualNode(proxy.$refs.container)) {
                    e.preventDefault();
                    e.stopPropagation();
                } else {
                    isShowMenu.value = false;
                }
            });
        });
        /**
         * H??m x??? l?? nh???n gi?? tr??? khi focus
         * @param {*} e 
         * @author DuongNhung
         */
        const onFocus = (e) => {
            proxy.forcused = true;
            emit("focus", proxy.disp, proxy.valueField, e);
        };
        /**
         * H??m x??? l?? nh???n gi?? tr??? khi out focus
         * @param {*} e 
         * @author DuongNhung
         */
        const onFocusOut = (e) => {
            proxy.forcused = false;
            emit("focusout", proxy.disp, proxy.valueField, e);
        };
        const eventListsioner = computed(() => {
            return {
                click: (e) => {
                    proxy.cancelEvent(e);
                    proxy.isShowMenu = !proxy.isShowMenu;
                },
                focusout: (e) => {
                    proxy.onFocusOut(e);
                },
                focus: (e) => {
                    proxy.cancelEvent(e);
                    proxy.onFocus(e);
                },
                change: (e) => {
                    proxy.cancelEvent(e);
                    // proxy.changeValue(e);
                },
                keydown: (e) => {
                    emit("keydown", e);
                },
                keyup: (e) => {
                    emit("keyup", e);
                },
            };
        });

        /**
         * Set width,height cho dropdown
         * Author: DuongNhung
         */
        function setDropdown() {
            let offset = proxy.$refs.dropdown.getBoundingClientRect();
            offsetDropdown.width = offset.width;
            offsetDropdown.height = 200.5 - this.heightCb;
        }

        /**
         *????ng menu dropdown khi windown click
         * Author: DuongNhung
         */
        function initEvent() {
            document.addEventListener("click", (e) => {
                if (proxy.isShowMenu) {
                    let target = e.target;
                    let cbo =
                        target.closest(".dropdown-menu") ||
                        target.closest(".dropdown-menu-toggle");
                    if (!cbo) {
                        proxy.isShowMenu = false;
                    }
                }
            });
        }

        return {
            itemClick,
            cancelEvent,
            eventListsioner,
            setPosition,
            setDropdown,
            style,
            isShowMenu, //Show menu dropdown
            display,
            selected,
            initEvent, //????ng menu dropdown khi windown click,
            search,
            data,
            disp, // G??n gi?? tr???
            autoHeight,
            onFocusOut,
            forcused,
            onFocus


        };
    },
};
</script>
<style lang="scss" scoped>
@import "@/style/components/MsDropDown.scss";
</style>
  