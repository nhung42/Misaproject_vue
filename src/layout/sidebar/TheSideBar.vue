<template>
    <div class="wrap-left" :class="{ 'menu-hide': collapsed }" :style="{ width: sideBarWidth }">
        <div class="wrap-logo">
            <a class="icon-9-cham">
            </a>
            <a class="logo">
                <img src="@/assets/img/Logo_Module_TiengViet_White.66947422.svg" height="32" width="96" />
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
import { ref, getCurrentInstance, computed, onMounted, watch } from "vue";
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

        const menuItems = computed(() => {
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
                    text: "T???ng quan",
                    arrow: "",
                    title: "T???ng quan",
                },
                {
                    path: "/employee",
                    icon: "icon-tien-mat",
                    text: "Ti???n m???t",
                    arrow: "",
                    title: "Ti???n m???t",
                },
                {
                    path: "",
                    icon: "icon-hop-dong",
                    text: "H???p ?????ng",
                    arrow: "",
                    title: "H???p ?????ng",
                },
                {
                    path: "",
                    icon: "icon-mua-hang",
                    text: "Mua h??ng",
                    arrow: "",
                    title: "Mua h??ng",
                },
                {
                    path: "",
                    icon: "icon-ban-hang",
                    text: "B??n h??ng",
                    arrow: "",
                    title: "B??n h??ng",
                },
                {
                    path: "",
                    icon: "icon-qlhd",
                    text: "Qu???n l?? ho?? ????n",
                    arrow: "",
                    title: "Qu???n l?? ho?? ????n",
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
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;500;700&display=swap');
</style>
  