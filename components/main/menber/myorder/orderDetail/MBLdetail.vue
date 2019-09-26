<template>
    <div v-if="loadbox">
        <div v-if="nodate" class="recommend-content">
            <div>
                <div class="module">
                    <ul class="clearfix">
                        <li><span class="fw">WWL NO：</span> {{mbl.workNo}}</li>
                        <li><span class="fw">M/B L：</span> {{mbl.mblNo}}</li>
                        <li><span class="fw">{{$t('common.shipCom')}}：</span> {{mbl.shippingCode}}</li>
                        <li><span class="fw">{{$t('common.Carrier')}}：</span> {{mbl.vessel}}</li>
                        <li><span class="fw">{{$t('common.Vessel')}}：</span> {{mbl.voyage}}</li>
                        <li><span class="fw">{{$t('common.PlaceOfReceipt')}}：</span> {{mbl['placeDeliveryEn']}}</li>
                        <li><span class="fw">{{$t('common.POL')}}：</span> {{mbl['portStartEn']}}</li>
                        <li><span class="fw">{{$t('common.POD')}}：</span> {{mbl['portEndEn']}}</li>
                        <li><span class="fw">{{$t('common.PlaceofDelivery')}}：</span> {{mbl['placeReceiptEn']}}</li>
                        <li><span class="fw">ETD：</span> {{mbl.etd | dateInit('MM-DD-YYYY',language)}}</li>
                        <li><span class="fw">{{$t('common.Paymentmethod')}}：</span> {{mbl['payMethod'+language]}}</li>
                        <li><span class="fw">{{$t('common.Signingmethod')}}：</span> {{mbl['blMenthod'+language]}}</li>
                        <li><span class="fw">{{$t('common.Transportation')}}：</span> {{mbl['transportClause'+language]}}</li>
                    </ul>
                </div>

                <div class="module">
                    <p class="border-bottom tit-pic">
                        <!-- <img src="../../../../../../static/images/icon/order/partner.png" alt="" srcset=""> -->
                        <i class="iconfont icon-hezuo"></i>
                        {{$t('common.Parties')}}
                    </p>
                    <ul class="clearfix">
                        <li class="tit">{{$t('order.Consigner')}}： </li>
                        <!-- <li>{{$t('common.Name')}}： {{mbl.fName}}</li> -->
                    </ul>
                    <div class="text-area bor-b">
                        <div class="textarea">
                            {{mbl.fAddress}}
                        </div>
                    </div>
                    <ul class="clearfix">
                        <li class="tit">{{$t('order.Consignee')}}： </li>
                        <!-- <li>{{$t('common.Name')}}： {{mbl.sName}}</li> -->
                    </ul>
                    <div class="text-area bor-b">
                        <div class="textarea">
                            {{mbl.sAddress}}
                        </div>
                    </div>
                    <ul class="clearfix">
                        <li class="tit">{{$t('common.Notify')}}： </li>
                        <!-- <li>{{$t('common.Name')}}： {{mbl.tName}}</li> -->
                    </ul>
                    <div class="text-area bor-b">
                        <div class="textarea">
                            {{mbl.tAddress}}
                        </div>
                    </div>
                    <ul class="clearfix">
                        <li class="tit">{{$t('common.Secondnotify')}}： </li>
                        <!-- <li>{{$t('common.Name')}}： {{mbl.tNameSecond}}</li> -->
                    </ul>
                    <div class="text-area bor-b">
                        <div class="textarea">
                            {{mbl.tAddressSecond}}
                        </div>
                    </div>
                </div>

                <div class="module">
                    <div class="tit-table">{{$t('common.typevolume')}}</div>
                    <div class="content" ref="content">
                        <div class="long">
                            <table border="1" >
                                <thead>
                                    <tr>
                                        <th width="100">{{$t('common.Boxtype')}}</th>
                                        <th width="100">{{$t('common.Quantity')}}</th>
                                        <th width="100">{{$t('common.BoxNo')}}</th>
                                        <th width="100">{{$t('common.Sealno')}}</th>
                                        <!-- <th width="100">{{$t('common.SOC')}}</th> -->
                                        <!-- <th width="100">{{$t('common.remark')}}</th> -->
                                        <th width="100">{{$t('common.jianshu')}}</th>
                                        <th width="100">{{$t('common.Packing')}}</th>
                                        <th width="100">{{$t('common.Weight')}}</th>
                                        <th>{{$t('common.size')}}</th>
                                    </tr>
                                </thead>
                                <tbody>
                                    <tr v-for="(item,index) in mblNumList" :key="index">
                                        <td>{{item['ctnType'+language]}}</td>
                                        <td>{{item.number}}</td>
                                        <td>{{item.ctnNo}}</td>
                                        <td>{{item.sealNo}}</td>
                                        <!-- <td>{{item['socFlag'+language]}}</td> -->
                                        <!-- <td>{{item.remark}}</td> -->
                                        <td>{{item.jianshu}}</td>
                                        <td>{{item['packageType'+language]}}</td>
                                        <td>{{item.weight}}</td>
                                        <td>{{item.size}}</td>
                                    </tr>
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>

                <div class="module">
                    <p class="border-bottom tit-pic">
                        <!-- <img src="../../../../../../static/images/icon/order/hwxx.png" alt="" srcset=""> -->
                        <i class="iconfont icon-dingdan"></i>
                        {{$t('common.CargoInformation')}}
                    </p>
                    <ul class="clearfix">
                        <li class="tit">{{$t('common.Shippingmark')}}： </li>
                    </ul>
                    <div class="text-area bor-b">
                        <div class="textarea">
                            {{mblGoods.marks}}
                        </div>
                    </div>
                    <ul class="clearfix">
                        <li class="tit">{{$t('common.Tradename')}}： </li>
                    </ul>
                    <div class="text-area bor-b">
                        <div class="textarea">
                            {{mblGoods.goods}}
                        </div>
                    </div>
                    <ul class="clearfix">
                        <li class="tit">{{$t('common.remark')}}： </li>
                    </ul>
                    <div class="text-area bor-b">
                        <div class="textarea">
                            {{mblGoods.remark}}
                        </div>
                    </div>
                    <ul class="clearfix">
                        <li><span class="fw">{{$t('order.goodsType')}}：</span> {{mblGoods['goodsType'+language]}}</li>
                        <li><span class="fw">{{$t('order.Number')}}：</span> {{mblGoods.number}}</li>
                        <li><span class="fw">{{$t('order.packtype')}}：</span> {{mblGoods['packageType'+language]}}</li>
                        <li><span class="fw">{{$t('order.weigth')}}：</span> {{mblGoods.weight}}KGS</li>
                        <li><span class="fw">{{$t('order.Volume')}}：</span> {{mblGoods.volume}}CBM</li>
                        <div v-if="mblGoods.goodsType==2">
                            <li><span class="fw">{{$t('order.wxlb')}}：</span> {{mblGoods.dgType}}</li>
                            <li><span class="fw">{{$t('order.UNNumber')}}：</span> {{mblGoods.unNo}}</li>
                            <li><span class="fw">{{$t('order.wxym')}}：</span> {{mblGoods.dgPageNo}}</li>
                            <li><span class="fw">{{$t('order.wpsd')}}：</span> {{mblGoods.flashPoint}}</li>
                            <li><span class="fw">PACKING GROUP：</span> {{mblGoods.packingGroup}}</li>
                        </div>
                        <div v-if="mblGoods.goodsType==3">
                            <li><span class="fw">{{$t('order.Temperature')}}：</span> {{mblGoods.temperature}}</li>
                            <li><span class="fw">{{$t('order.Ventilation')}}：</span> {{mblGoods.ventilation}}</li>
                            <li><span class="fw">{{$t('order.Luggage')}}：</span> {{mblGoods.containerStuffingDate}}</li>
                        </div>
                        <div v-if="mblGoods.goodsType==4">
                            <li><span class="fw">{{$t('order.Length')}}：</span> {{mblGoods.length}}CM</li>
                            <li><span class="fw">{{$t('order.Width')}}：</span> {{mblGoods.width}}CM</li>
                            <li><span class="fw">{{$t('order.Height')}}：</span> {{mblGoods.height}}CM</li>
                        </div>
                    </ul>
                </div>

                <!-- <div class="module">
                    <p class="border-bottom tit-pic">
                        <i class="iconfont icon-dingdan"></i>
                        {{$t('common.Documentaryinfo')}}
                    </p>
                    <ul class="clearfix">
                        <li>{{$t('common.Documentary')}}：{{mbl.contract}}</li>
                        <li>{{$t('order.tel')}}：{{mbl.tel}}</li>
                        <li>{{$t('setting.email')}}：{{mbl.email}}</li>
                    </ul>
                </div> -->

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
                mbl:{},
                mblGoods:{},
                mblNumList:[],
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
                // let {id} = this.$route.query
                toDetailBooking('toDetailBookingMbl',{bookingId:id}).then(res=>{
                    let {mbl,mblGoods,mblNumList} = res.data;
                    this.mbl = mbl||{};
                    this.mblGoods = mblGoods||{};
                    this.mblNumList = mblNumList||[];
                    this.loadbox = true;
                    if(!Object.keys(this.mbl).length&&!Object.keys(this.mblGoods).length&&!this.mblNumList.length){
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
                font-size: .3rem; color: #3291ef; margin-right: 8px;
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
                background: #eee; min-height: 1.8rem; color: #666;
            }
        }
    }
    .tit-table{
        line-height:.6rem; background: #dceffe; font-size: .26rem; text-align: center;
    }
    table{
        border-collapse:collapse; font-size: .24rem; color: #666; border:1px solid #cdcccc; table-layout: fixed; width: 1000px;
        th,td{
            box-sizing: border-box; padding: .15rem 0; text-align: center;
        }
    }
    .content{
        overflow: hidden;width: 100%;
        .long{
            width: 1000px;
        }
    }
</style>