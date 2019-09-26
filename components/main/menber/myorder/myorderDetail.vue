<template>
    <div class="fcl-detail" id="fcl-detail">
        <nav-bar :title="$t('order.orderDetail')"></nav-bar>
        <foutdetail :paramsparent="params"></foutdetail>
    </div>
</template>

<script>
    import navBar from '@/components/common/navBar'
    import foutdetail from './detailfour.vue';
    export default {
        components:{
            foutdetail,
            navBar
        },
        created(){
            this.params = this.$route.query
        },
    }
</script>

<style lang="less" scoped>
    .fcl-detail{
        background: #eee; padding-top: 46px; padding-bottom: 60px; box-sizing: border-box;  height: 100%; overflow: hidden; 
    }
</style>