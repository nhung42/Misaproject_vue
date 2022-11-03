<template>
    <div class="wrap-left" :class="{ 'menu-hide': collapsed }" :style="{ width: sideBarWidth }">
        <div class="wrap-logo">
            <a class="icon-9-cham">
            </a>
            <a class="logo">
                <img src="@/assets/img/Logo_Module_TiengViet_White.66947422.svg" height="32px"
                    width="96px" />
            </a>
        </div>
        <div class="menu">
            <div class="menu-group">
                <div v-for="(item, index) in menuItems" :key="index">
                    <ms-tooltip :content="collapsed ? item.title : ''" placement="right" right="right">
                        <a class="menu-item" :href="item.path" :class="[item.path == activeItem?.path ? 'active' : '']"
                            @click="() => clickMenu(item)">
                            <div :class="['app-icon', item.icon]"></div>
                            <span class="text-menu">{{ item.text }}</span>
                            <div class="arrow-menu misa-icon app-icon" :class="item.arrow"></div>
                        </a>
                    </ms-tooltip>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
import {ref, getCurrentInstance ,computed,onMounted,watch} from "vue";
import MsTooltip from "@/components/ms-control/tooltip/MsTooltip.vue";
import { collapsed, sideBarWidth } from "./State";
export default {
    name: "TheLeftMenu",
    components: { MsTooltip },
    data() {
        return {};
    },
    computed: {
      
    },

    setup(props, { emit }) {
        const { proxy } = getCurrentInstance();

        const isActive = ref(false);

        const activeItem = ref(null);

        // function setActive() {
        //     proxy.isActive = true;
        // }
        onMounted(() => {
            proxy.activeMenu();
        });

        const   menuItems = computed(()=>{
            return proxy.getMenuItems();
        }) 
        watch(
            () => activeItem.value,
            (newVal) => {
                if (newVal) {
                    proxy.activeMenu();
                }
            }
        );

        function activeMenu() {
            let pathName = location.pathname.toLowerCase();
            let item = proxy.menuItems.find((x) => x.path == pathName);
            proxy.activeItem = item || proxy.menuItems[1];
        }

        const cancelEvent = (e) => {
            if (e) {
                if (typeof e.preventDefault === "function") {
                    e.preventDefault();
                }
                if (typeof e.stopPropagation === "function") {
                    e.stopPropagation();
                }
                if (typeof e.stopImmediatePropagation === "function") {
                    e.stopImmediatePropagation();
                }
            }
        };

        const Listsioner = computed(() => {
            // const me = this;
            return {
                click: (event) => {
                    cancelEvent(event);
                },
                mousedown: (event) => {
                    cancelEvent(event);
                },
                keydown: (event) => {
                    cancelEvent(event);
                    emit("keydown", event);
                },
                focus: (event) => {
                    cancelEvent(event);
                    emit("keydown", event);
                },
            };
        });
        const getMenuItems = () => {
            let menuItems = [
                {
                    path: "/dashboard",
                    icon: "icon-tong-quan",
                    text: "Tổng quan",
                    arrow: "",
                    title: "Tổng quan",
                },
                {
                    path: "/employee",
                    icon: "icon-tien-mat",
                    text: "Tiền mặt",
                    arrow: "",
                    title: "Tiền mặt",
                },
                {
                    path: "",
                    icon: "icon-hop-dong",
                    text: "Hợp đồng",
                    arrow: "",
                    title: "Hợp đồng",
                },
                {
                    path: "",
                    icon: "icon-mua-hang",
                    text: "Mua hàng",
                    arrow: "",
                    title: "Mua hàng",
                },
                {
                    path: "",
                    icon: "icon-ban-hang",
                    text: "Bán hàng",
                    arrow: "",
                    title: "Bán hàng",
                },
                {
                    path: "",
                    icon: "icon-qlhd",
                    text: "Quản lí hoá đơn",
                    arrow: "",
                    title: "Quản lí hoá đơn",
                },
                {
                    path: "",
                    icon: "icon-kho",
                    text: "Kho",
                    arrow: "",
                    title: "Kho",
                },
            ];
            return menuItems;
        };

        function clickMenu(item) {
            proxy.$router.push(item.path);
        }

        return {
            getMenuItems,
            collapsed,
         
            sideBarWidth,
            Listsioner,
            isActive,
            clickMenu,
            activeItem,
            activeMenu,
            menuItems
        };
    },
};
</script>
<style lang="scss" scoped>
@import "@/style/common/color.scss";
@import "@/style/common/TheLeftMenu.scss";
</style>
  