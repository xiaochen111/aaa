<template>
    <div >

        <div v-if="act==0" >
            <order-info :params="paramsparent"></order-info>
        </div>
        <div v-if="act==1">
            <mbl-detail :params="paramsparent"></mbl-detail>
        </div>
        <div v-if="act==2">
            <hbl-detail :params="paramsparent"></hbl-detail>
        </div>
        <div v-if="act==3">
            <prewired-detail :params="paramsparent"></prewired-detail>
        </div>
        <div v-if="act==4">
            <tracking :params="paramsparent"></tracking>
        </div>
        
        <ul class="tab-bot">
            <li @click="navChange(0)" :class="act==0?'act':''">
                <i class="iconfont icon-dingdanxinxi"></i>
                {{$t("common.orderInfo")}}
            </li>
            <li @click="navChange(1)" :class="act==1?'act':''" v-if="loginflag.infor.bussiType!=2">
                <i class="iconfont icon-MBL"></i>
                MBL
            </li>
            <li @click="navChange(2)" :class="act==2?'act':''">
                <i class="iconfont icon-HBL"></i>
                HBL
            </li>
            <li @click="navChange(3)" :class="act==3?'act':''" v-if="loginflag.infor.bussiType!=3">
                <i class="iconfont icon-yupeicangdan"></i>
                {{$t("common.manifest")}}
            </li>
            <li @click="navChange(4)" :class="act==4?'act':''">
                <i class="iconfont icon-VGM"></i>
                货物跟踪
            </li>
        </ul>

    </div>
</template>

<script>
    import orderInfo from '@/components/main/menber/myorder/orderDetail/orderInfo'
    import mblDetail from '@/components/main/menber/myorder/orderDetail/MBLdetail'
    import hblDetail from '@/components/main/menber/myorder/orderDetail/HBLdetail'
    import prewiredDetail from '@/components/main/menber/myorder/orderDetail/prewiredDetail'
    import vgmDetail from '@/components/main/menber/myorder/orderDetail/vgmDetail'
    import tracking from '@/components/main/menber/myorder/orderDetail/tracking'
    import {mapState} from 'vuex'
    export default {
        components:{
            orderInfo,
            mblDetail,
            hblDetail,
            prewiredDetail,
            vgmDetail,
            tracking,
        },
        props:['paramsparent'],
        data(){
            return {
               act:10
            }
        },
        mounted() {
            console.log(this.paramsparent)
            this.act = this.paramsparent.act || 0;
        },
        methods:{
            navChange(num){
                this.act = num;
            }
        },
        computed:{
            ...mapState(['language','loginflag']),  //这个是后台中英文翻译的关键
        },
    }
</script>

<style lang="less" scoped>
   
    .tab-bot{
        height: .9rem; background: #fff; position: fixed; bottom: 0; width: 100%; box-shadow: 0 -10px 13px -10px rgba(0,0,0,0.35);
        display: flex; align-items: center; justify-content: space-around;
        li,a{
            font-size: .2rem; color: #666;  text-align: center; display: flex; width: 100%/3;
            align-items: center; flex-direction: column;
            img{
                width: .4rem; height: .4rem;
            }
        }
        i{
            font-size: .4rem;
        }
        li.act{
            color:#1d89e4;
        }
    }
</style>