<template>
    <div>
        <nav class="mui-bar mui-bar-tab">
            <router-link to="/home" class="mui-tab-item" href="#tabbar">
                <span class="mui-icon mui-icon-home"></span>
                <span class="mui-tab-label">首页</span>
            </router-link>
            <router-link to="/member" class="mui-tab-item" href="#tabbar-with-chat">
                <span class="mui-icon mui-icon-member"></span>
                <span class="mui-tab-label">会员</span>
            </router-link>
            <router-link to="/shopcart" class="mui-tab-item" href="#tabbar-with-contact">
                <span class="mui-icon mui-icon-shopcart"><span class="mui-badge">{{getNum}}</span></span>
                <span class="mui-tab-label">购物车</span>
            </router-link>
            <router-link to="/search" class="mui-tab-item" href="#tabbar-with-map">
                <span class="mui-icon mui-icon-search"></span>
                <span class="mui-tab-label">查找</span>
            </router-link>
        </nav>
    </div>
</template>
<script>
import Connect from '../commons/Connect.js';
import GoodsTools from '../goods/GoodsTools.js';

export default {
    data() {
        return {}
    }, computed: {
        getNum() {
            return this.$store.getters.getProdsCount;
        }
    }
}
</script>
<style>
</style>
