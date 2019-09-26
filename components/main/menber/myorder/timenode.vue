<template>
    <div id="time-node" v-if="loadflag">
        <van-nav-bar :title="$t('order.CargoTracking')" :left-text="$t('common.goback')" left-arrow fixed  @click-left="onClickLeft">
        </van-nav-bar>
        <div v-if="nodate" class="noDate">
            <svg class="icon" aria-hidden="true">
                <use xlink:href="#icon-zanwushuju"></use>
            </svg>
            <p>{{$t('common.nodata')}}</p>
        </div>
        <scroll class="recommend-content" v-else>
            <div>
                <div class="module">
                    <ul class="clearfix">
                        <li><span class="fw">{{$t('order.morder')}}：</span>  {{mblNo}}</li>
                        <li><span class="fw">{{$t('common.POL')}}：</span>  {{order.portStartIdEn}}</li>
                        <li><span class="fw">{{$t('common.POD')}}：</span>  {{order.portEndIdEn}}</li>
                        <li><span class="fw">{{$t('common.shipCom')}}：</span>  {{order.shippingIdCode}} </li>
                    </ul>
                </div>
                <div class="tab">
                    <p @click="tabindex=0" :class="tabindex==0?'act':''">{{$t('order.bookTrack')}}</p>
                    <p @click="tabindex=1" :class="tabindex==1?'act':''">{{$t('order.containerInfo')}}</p>
                </div>
                <div class="module-time-box" v-if="tabindex == 0">
                    <p class="tit-color">{{$t('order.boatInfo')}}</p>
                    <ul class="clearfix">
                        <li><span class="fw">{{$t('order.vessel')}}：</span>  {{order.vessel}}</li>
                        <li><span class="fw">{{$t('order.voyage')}}：</span>  {{order.voyage}}</li>
                    </ul>
                    <div class="time-node">
                        <p :class="trackingSeaBill.bkgTime?'border-bottom act':'border-bottom'">BKG: {{trackingSeaBill.bkgTime|dateInit('MM-DD-YYYY hh:mm')}}</p>
                        <p :class="trackingSeaBill.etdTime?'border-bottom act':'border-bottom'">ETD: {{trackingSeaBill.etdTime|dateInit('MM-DD-YYYY hh:mm')}}</p>
                        <p :class="trackingSeaBill.atdTime?'border-bottom act':'border-bottom'">ATD: {{trackingSeaBill.atdTime|dateInit('MM-DD-YYYY hh:mm')}}</p>
                        <p :class="trackingSeaBill.etaTime?'border-bottom act':'border-bottom'">ETA: {{trackingSeaBill.etaTime|dateInit('MM-DD-YYYY hh:mm')}}</p>
                        <p :class="trackingSeaBill.ataTime?'border-bottom act':'border-bottom'">ATA: {{trackingSeaBill.ataTime|dateInit('MM-DD-YYYY hh:mm')}}</p>
                    </div>
                </div>
                

                <div v-if="tabindex == 1">
                    <van-collapse v-model="activeNames" accordion>
                        <van-collapse-item  :name="index" v-for="(item,index) in voList" :key="index" >
                            <div class="title-item" slot="title">
                                <span>{{$t('order.cartonNo')}}: {{item.ctnNo}}</span>
                                <span>{{$t('order.boxpile')}}: {{item.ctnTypeCn}}</span>
                            </div>
                            <div class="time-node mt0">
                                <p :class="item.arrivalTime?'border-bottom act':'border-bottom'">{{$t('order.Heavycontainerentry')}}: {{item.arrivalTime|dateInit('MM-DD-YYYY hh:mm')}}</p>
                                <p :class="item.vgmTime?'border-bottom act':'border-bottom'">{{$t('order.VGMhasarrived')}}: {{item.vgmTime|dateInit('MM-DD-YYYY hh:mm')}}</p>
                                <p :class="item['terminalRlsFlag'+language]?'border-bottom act':'border-bottom'">{{$t('order.Dockrelease')}}: {{item['terminalRlsFlag'+language]}}</p>
                                <p :class="item['customsRlsFlag'+language]?'border-bottom act':'border-bottom'">{{$t('order.Customsclearance')}}: {{item['customsRlsFlag'+language]}}</p>
                                <p :class="item.departureTime?'border-bottom act':'border-bottom'">{{$t('order.Shippedoutofport')}}: {{item.departureTime|dateInit('MM-DD-YYYY hh:mm')}}</p>
                            </div>
                        </van-collapse-item>
                        
                    </van-collapse>
                </div>
                

            </div>
        </scroll>
    </div>
</template>

<script>
    import { NavBar,Collapse, CollapseItem ,} from 'vant';
    // import {toBookingTrackingdetailDef} from '@/api/getData'
    import { toDetailBooking } from '@/api/sysapi';
    import {mapState} from 'vuex'
    import Scroll from '@/components/common/scroll'
    export default {
        components:{
            [NavBar.name]:NavBar,
            [Collapse.name]:Collapse,
            [CollapseItem.name]:CollapseItem,
            Scroll
        },
        data(){
            return {
                order:{},
                trackingSeaBill:{},
                voList:[],
                mblNo:'',
                tabindex:0,
                activeNames: 0,
                loadflag:false,
                nodate:true
            }
        },
        created() {
            let {id,params} = this.$route.query;
            if(id){
                this.toBookingTrackingFn('toBookingTracking',{bookingId:id})
            }else{
                this.toBookingTrackingFn('indexToBookingTracking',{params})  //首页跳转过来查询
            }
        },
        computed:{
            ...mapState(['language']),  //这个是后台中英文翻译的关键
        },
        methods:{
            onClickLeft(){
                history.go(-1);
            },
            toBookingTrackingFn(url,params){
                toDetailBooking(url,params).then(res=>{
                    this.loadflag = true;
                    if(!res.data){
                        return
                    }
                    let {order,trackingSeaBill,voList,mblNo} = res.data;
                    this.order = order || {};
                    this.trackingSeaBill = trackingSeaBill || {};
                    this.voList = voList || [];
                    this.mblNo = mblNo;
                    this.nodate = false
                })
            }
        }
    }
</script>

<style lang="less" scoped>
    #time-node{
        padding-top: 46px; box-sizing: border-box; overflow: hidden; height: 100%;
    }
    .module,.module-time-box{
        box-sizing: border-box; padding:.1rem .2rem; background: #fff; margin-bottom: .2rem;
        ul{
            li{
                display: flex; align-items: center; font-size: .24rem; color: #666; box-sizing: border-box; padding: 5px 0;
                span.fw{
                    font-weight: bold;
                }
            }
        }
    }
    .tab{
        display: flex; align-items: center; background: #fff; box-sizing: border-box; padding: 10px 0;margin-bottom: .1rem;
        p{
            font-size: .26rem; color: #666; text-align: center; flex-grow: 1;
            &:first-child{
                border-right: 1px solid #ccc;
            }
            &.act{
                color: #3291ef; font-weight: bold;
            }
        }
    }
    .module-time-box{
        .tit-color{
            text-indent: 10px; line-height: 30px; font-size: .24rem; color: #3291ef;position: relative;
            &:after{
                content: '';display: block; position: absolute; height: 20px; width: 3px; background: #3291ef; left: 0; top: 5px;
            }
        }
    }
    .time-node{
        margin-left: 20px;  border-left: 1px solid #ccc; margin-top: .13rem;
        &.mt0{
            margin-top: 0;
        }
        p{
            line-height: .75rem; font-size: .24rem; color: #666;margin-left: 15px; position: relative;
            &:before{
                content: ''; display: block; height: 10px; width: 10px; border-radius: 50%; position: absolute;
                left: -20px; top: 50%; transform: translateY(-50%); background: #ccc;
            }
            &.act{
                color: #3291ef;
                &:before{
                    content: ''; display: block; height: 16px; width: 16px; border-radius: 50%; position: absolute;
                    left: -23px; top: 50%; transform: translateY(-50%); background: #3291ef;
                }
            }
        }
    }
    .title-item{
        display: flex; align-items: center;
        span{
            flex-grow: 1;
        }
    }
    .noDate{
        height: 100%; text-align: center;
        svg{
            width: 4.5rem; height: 4.5rem;
        }
        p{
            color: #9ac5e2; font-size: .22rem; text-align: center;
        }
    }
</style>