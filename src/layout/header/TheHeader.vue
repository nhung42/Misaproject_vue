<template>
    <div class="app-header">
        <div class="nav">
            <div class="nav__left">
                <div class="icon-stripes" id="sidebar_dong"></div>
                <div class="name-company"> CÔNG TY TNHH SẢN XUẤT-THƯƠNG MẠI-DỊCH VỤ QUI PHÚC</div>
                <div class="icon-down"></div>
            </div>
            <div class="nav__right">
                <div class="icon-bell"></div>
                <div class="profile">
                    <div class="icon-user"></div>
                    <div class="name">Nguyễn Văn Mạnh</div>
                    <div class="icon-drow-user"></div>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
export default {
}
</script>
<style lang="scss">
@import "@/style/common/icon.scss";
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;500;700&display=swap');

.app-header {
    height: 56px;
    width: calc(100% - 200px);
    float: right;
    font-family: Roboto;

    .nav {
        display: flex;
        justify-content: space-between;
        height: 56px;
        box-sizing: border-box;
        flex: 1;
        position: relative;
        align-items: center;
        flex-wrap: nowrap;
    }

    .nav__left {
        display: flex;
    }

    .nav__right {
        display: flex;
    }

    .name-company {
        height: 56px;
        line-height: 56px;
        margin-right: 10px;
        margin-left: 8px;
        font-weight: 600;
        font-size: 18px;
    }

    .profile {
        display: flex;
        justify-content: flex-end;
        padding-right: 14px;
        font-weight: 400;
    }

    .name {
        margin-left: 10px;
        line-height: 36px;
        opacity: 0.7;
        margin-right: 8px;
    }
}
</style>