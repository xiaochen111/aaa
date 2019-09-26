<template>
<div v-if="loadbox">
    <div v-if="nodate"  class="recommend-content">
        <div>
            <div class="module">
                <ul class="clearfix">
                    <li><span class="fw">{{$t('common.WorkNo')}}：</span> {{prewiredInfo.workNo}}</li>
                    <li><span class="fw">{{$t('common.Carrier')}}：</span> {{prewiredInfo.vessel}}</li>
                    <li><span class="fw">{{$t('common.Vessel')}}：</span> {{prewiredInfo.voyage}}</li>
                    <li><span class="fw">ETD：</span> {{prewiredInfo.etd | dateInit('MM-DD-YYYY',language)}}</li>
                    <li><span class="fw">{{$t('common.POL')}}：</span> {{prewiredInfo['portStartEn']}}</li>
                    <li><span class="fw">{{$t('common.POD')}}：</span> {{prewiredInfo['portEndEn']}}</li>
                </ul>
            </div>

            <div class="module">
                <p class="border-bottom tit-pic">
                    <i class="iconfont icon-hezuo"></i>
                    {{$t('common.Parties')}}
                </p>
                <ul class="clearfix">
                    <li class="tit">{{$t('order.Consigner')}}： </li>
                    <li><span class="fw">{{$t('common.Name')}}：</span> {{prewired.sName}}</li>
                    <li><span class="fw">{{$t('common.address')}}：</span> {{prewired.sAddress}}</li>
                    <li><span class="fw">{{$t('common.countries')}}：</span> {{prewired.sCountryCode}}</li>
                    <li><span class="fw">{{$t('common.Enterprisecode')}}：</span> {{prewired.sRegistrationCode}}</li>
                    <li><span class="fw">{{$t('order.tel')}}：</span> {{prewired.sTel}}</li>
                    <li><span class="fw">{{$t('setting.email')}}：</span> {{prewired.sEMail}}</li>
                    <li><span class="fw">{{$t('common.fax')}}：</span> {{prewired.sFax}}</li>
                </ul>
                <div class="text-area bor-b">
                    <!-- <div class="textarea">
                    
                    </div> -->
                </div>
                <ul class="clearfix">
                    <li class="tit">{{$t('order.Consignee')}}： </li>
                    <li><span class="fw">{{$t('common.Name')}}：</span> {{prewired.cName}}</li>
                    <li><span class="fw">{{$t('common.address')}}：</span> {{prewired.cAddress}}</li>
                    <li><span class="fw">{{$t('common.countries')}}：</span> {{prewired.cCountryCode}}</li>
                    <li><span class="fw">{{$t('common.Enterprisecode')}}：</span> {{prewired.cRegistrationCode}}</li>
                    <li><span class="fw">{{$t('order.tel')}}：</span> {{prewired.cTel}}</li>
                    <li><span class="fw">{{$t('setting.email')}}：</span> {{prewired.cEMail}}</li>
                    <li><span class="fw">{{$t('common.fax')}}：</span> {{prewired.cFax}}</li>
                </ul>
                <div class="text-area bor-b">
                    <!-- <div class="textarea">
                    
                    </div> -->
                </div>
                <ul class="clearfix">
                    <li class="tit">{{$t('common.Notify')}}： </li>
                    <li><span class="fw">{{$t('common.Name')}}：</span> {{prewired.nName}}</li>
                    <li><span class="fw">{{$t('common.address')}}：</span> {{prewired.nAddress}}</li>
                    <li><span class="fw">{{$t('common.countries')}}：</span> {{prewired.nCountryCode}}</li>
                    <li><span class="fw">{{$t('common.Enterprisecode')}}：</span> {{prewired.nRegistrationCode}}</li>
                    <li><span class="fw">{{$t('order.tel')}}：</span> {{prewired.nTel}}</li>
                    <li><span class="fw">{{$t('setting.email')}}：</span> {{prewired.nEMail}}</li>
                    <li><span class="fw">{{$t('common.fax')}}：</span> {{prewired.nFax}}</li>
                </ul>
                <div class="text-area bor-b">
                    <!-- <div class="textarea">
                    
                    </div> -->
                </div>
            </div>

            <div class="module">
                <div class="tit-table">{{$t('common.CargoDetail')}}</div>
                <div class="content" ref="content">
                    <div class="long">
                        <table border="1" >
                            <thead>
                                <tr>
                                    <th width="200">M/B L</th>
                                    <th width="130">{{$t('common.BoxNo')}}</th>
                                    <th width="100">{{$t('common.Sealno')}}</th>
                                    <th width="100">{{$t('common.Boxtype')}}</th>
                                    <th width="100">{{$t('common.jianshu')}}</th>
                                    <th width="100">{{$t('common.Weight')}}</th>
                                    <th width="100">{{$t('common.Volume')}}</th>
                                    <th width="100">{{$t('common.Emptyweight')}}</th>
                                    <th width="100">SOC</th>
                                    <th width="100">{{$t('common.Typeseal')}}</th>
                                    <th width="100">{{$t('common.Seals')}}</th>
                                    <th width="100">{{$t('common.Boxsource')}}</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="(item,index) in pcList" :key="index">
                                    <td>{{item.mblNo}}</td>
                                    <td>{{item.ctnNo}}</td>
                                    <td>{{item.sealNo}}</td>
                                    <td>{{item['ctnType'+language]}}</td>
                                    <td>{{item.number}}</td>
                                    <td>{{item.weight}}</td>
                                    <td>{{item.volume}}</td>
                                    <td>{{item['bareWeight'+language]}}</td>
                                    <td>{{item['socFlag'+language]}}</td>
                                    <td>{{item['sealType'+language]}}</td>
                                    <td>{{item['sealContact'+language]}}</td>
                                    <td>{{item['containerSource'+language]}}</td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>


            <div class="module">
                <p class="border-bottom tit-pic">
                    <!-- <img src="../../../../../../static/images/icon/huowuxinxi.png" alt="" srcset="">  -->
                    <i class="iconfont icon-dingdan"></i>
                    {{$t('common.CargoInformation')}}
                </p>
                <ul class="clearfix">
                    <li><span class="fw">{{$t('common.Totalnumber')}}：</span> {{prewired.number}}</li>
                    <li><span class="fw">{{$t('common.Totalweight')}}：</span> {{prewired.weight}}</li>
                    <li><span class="fw">{{$t('common.Totalvolume')}}：</span> {{prewired.volume}}</li>
                    <li><span class="fw">{{$t('common.Packing')}}：</span> {{prewired['packageType'+language]}}</li>
                    <li><span class="fw">{{$t('common.Shippingmark')}}：</span> {{prewired.marks}}</li>
                    <li><span class="fw">{{$t('common.Tradename')}}：</span> {{prewired.goods}}</li>
                    <li><span class="fw">HS CODE：</span> {{prewired.hsCode}}</li>
                </ul>
            </div>

        </div>
    </div>
    <div v-else class="noDate">
        <svg class="icon" aria-hidden="true">
            <use xlink:href="#icon-zanwushuju"></use>
        </svg>
        <p>{{$t('common.nodata')}}</p>
    </div>
</div>
</template>

<script>
    import BScroll from 'better-scroll'
    import { toDetailBooking } from '@/api/sysapi';
    import {mapState} from 'vuex'
    export default {
        props:['params'],
        data(){
            return {
                prewiredInfo:{},
                prewired:{},
                pcList:[],
                nodate:true,
                loadbox:false,
            }
        },
        created() {
            this.init();
        },
        computed:{
            ...mapState(['language']),  //这个是后台中英文翻译的关键
        },
        methods:{
            initScrollX(){ //better-scroll 横线滚动条
                this.scroll=new BScroll(this.$refs.content, {
                    startX:0,
                    click:true,
                    scrollX:true,
                    scrollY:false,
                })
            },
            init(){
                let {id} = this.params;
                //  let {id} = this.$route.query
                toDetailBooking('toDetailBookingPrewired',{bookingId:id}).then(res=>{
                    this.prewiredInfo = res.data;
                    this.prewired = res.data.prewired||{};
                    this.pcList = res.data.pcList||[];
                    this.loadbox = true;
                    if(!Object.keys(this.prewired).length&&!this.pcList.length){
                        this.nodate = false
                    }else{
                        this.nodate = true
                        setTimeout(()=>{
                            this.scrolldef();
                            this.wrapper.refresh();
                        },20)
                    }
                })
            },
            scrolldef(){
                this.initScrollX();
                let wrapper = document.querySelector('.recommend-content');
                this.wrapper = new BScroll(wrapper)
            }
        }
    }
</script>

<style lang="less" scoped>
    .recommend-content{
        position: absolute; left: 0; right: 0;  height: calc(~'100% - 96px');
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
    .module{
        box-sizing: border-box; padding:.1rem .2rem; background: #fff; margin-top: .2rem;
        ul{
            li{
                display: flex; align-items: center; font-size: .24rem; color: #666; box-sizing: border-box; padding: 5px 0;
                span.fw{
                    font-weight: bold;
                }
                &.tit{
                    font-size: .26rem; font-weight: bold;
                }
                &.col-6{
                    width: 50%; float: left;
                }
                .font-red{
                    color: #f42434;
                }
            }
        }
        .tit-pic{
            display: flex; align-items: center;  font-size: .28rem;color: #3291ef; box-sizing: border-box; padding: 5px 0; line-height: 25px; margin-bottom: 5px;
            i{
                font-size: .3rem;color: #3291ef; margin-right: 8px;
            }
            img{
                width: 20px; margin-right: 5px;
            }
        }
        .text-area{
            &.bor-t{
                box-sizing: border-box; padding:.15rem 0; border-top: 1px dashed #ccc;
            }
            &.bor-b{
                box-sizing: border-box; padding:.15rem 0; border-bottom: 1px dashed #ccc; margin-bottom: .1rem;
            }
            .textarea{
                font-size: .24rem; outline: none; border: 1px solid #cdcccc; width: 100%; height:auto;border-radius: 4px; box-sizing: border-box; padding: 5px; display: block;
                background: #eee; min-height: 1.8rem;
            }
        }
    }
    .tit-table{
        line-height:.6rem; background: #dceffe; font-size: .26rem; text-align: center;
    }
    table{
        border-collapse:collapse; font-size: .24rem; color: #666; border:1px solid #cdcccc; table-layout: fixed; width: 1200px;
        th,td{
            box-sizing: border-box; padding: .15rem 0; text-align: center;
        }
    }
    .content{
        overflow: hidden;width: 100%;
        .long{
            width: 1330px;
        }
    }
</style>