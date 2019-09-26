<template>
    <div id="yunjia">
        <nav-bar title="运价搜索"></nav-bar>
        <div class="main-content">
            <div class="fcl" v-if="show=='fcl'">
                <fcl></fcl>
            </div>
        </div>
    </div>
</template>

<script>
    import navBar from '@/components/common/navBar'
    import Scroll from '@/components/common/scroll';
    import fcl from '@/components/main/enquiry/fcl/fclsearch'
    import {mapState, mapMutations, mapGetters} from 'vuex'
    import {permissionsDef} from '@/utils/common'
    export default {
        components:{
            fcl,Scroll,navBar,
        },
        data(){
            return {
                show:'fcl',
                fob:false,
            }
        },
        methods:{
            showFn(prames){
                this.show = prames
            },
        },
        created(){
            if(this.loginflag.flag){
                this.fob = permissionsDef('/web/freight/freightfclweb/queryFreightFclListForFob.html');
            }
        },
        computed:{
            ...mapState(['language','loginflag']),
        },
    }
</script>

<style lang="less" scoped>
    #yunjia{
       height: 100%; overflow: hidden; box-sizing: border-box; padding-top: 46px;
    }
    .fcl,.main-content{
        height: 100%;
    }
    .tabTop{
        height: 1rem; display: flex; justify-content:space-around;background: #2a99f3;
        li{
            font-size:.24rem;  color: #fff; background: #2a99f3; width: 100%/3; text-align: center;  line-height: 1; float: left;
            display: flex;justify-content: center; align-items:center;flex-direction:column;
            span{
                display: inline-block; line-height: .35rem;
            }
            i{
                font-size: .4rem; color: #fff;
            }
        }
    }
    .fcl,.lcl,.air{
        position: relative;
    }
    .lcl:after{
        left: calc(100%/2 - .13rem);
    }
    .air:after{
        left: calc(100%/6*5 - .13rem);
    }
</style>