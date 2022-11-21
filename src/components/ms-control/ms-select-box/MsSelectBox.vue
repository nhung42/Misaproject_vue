<template>
    <div class="ms-select ms-flex" :value="selectedItem">
        <div class="selectedItem ms-flex" :id="id" @click="isShow = !isShow">
            {{ selectedItem }}
            <div v-if="!iconLagre" class="mi-chevron-small ml-2"></div>
            <div v-if="iconLagre" class="combobox__btn"></div>
        </div>
        <div v-if="isShow" class="option pa-2" :style="{ width: width + 'px' }">
            <div class="option__text" v-for="(item, index) in data" :key="index"
                :class="{ isSelected: item.value == selectedItem }" @click="selectItem(item.value)">
                <div>
                    {{ item.value }}
                </div>
                <div v-if="item.value == selectedItem && iconLagre"></div>
            </div>
        </div>
    </div>
</template>
  
<script>
export default {
    name: "ms-select",
    components: {},
    props: {
        data: {
            type: Array,
        },
        width: {
            type: String,
        },
        iconLagre: {
            type: Boolean,
            default: false,
        },
        id: {
            type: String,
            require: true,
        },
    },
    created() {
        window.addEventListener("click", this.checkClickOn);
    },
    // beforeDestroy() {
    //     window.removeEventListener("click", this.checkClickOn);
    // },
    data() {
        return {
            selectedItem: this.data[0].value,
            isShow: false,
        };
    },
    methods: {
        /**
         * Nhận giá trị được chọn
         * @param {*} value 
         * @author DuongNhung
         */
        selectItem(value) {
            this.selectedItem = value;
            this.isShow = false;
            this.$emit("getSelectValue", value);
        },
        /** 
         * Đóng option
        */
        checkClickOn(event) {
            if (!document.getElementById(this.id).contains(event.target)) {
                this.isShow = false;
            }
        },
    },
};
</script>
  
<style scoped>
.ms-select {
    position: relative;
    font-size: 14px;
    font-weight: bold;
    cursor: pointer;
}

.selectedItem {
    width: 100%;
}

.combobox__btn {
    width: 36px;
    height: 36px;
    font-size: 20px;
    border-radius: 4px;
    outline: unset;
    border: none;
    position: absolute;
    right: 10px;
    background-color: unset;
    cursor: pointer;
    background-image: url(/src/assets/img/Sprites.64af8f61.svg);
    background-position: -1800px -478px;
    width: 8px;
    height: 5px;
    margin-top: -12px;
}

.ms-select .mi-chevron-down-white {
    background: #666;
}


.option {
    position: absolute;
    width: max-content;
    bottom: 40px;
    right: 0;
    border-radius: 4px;
    z-index: 999999999999999999;
    border: 1px solid #afafaf;
    background-color: #fff;
    box-shadow: 0 0 4px rgba(0, 0, 0, 0.2);
}

.option__text {
    font-size: 14px;
    font-weight: normal;
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 0 8px 0 8px;
    height: 37px;
    border-radius: 4px;
    cursor: pointer;
}

.option__text:hover {
    background-color: #a2f399;
}

.ms-select .isSelected {
    background-color: #a2f399;
}
</style>
  