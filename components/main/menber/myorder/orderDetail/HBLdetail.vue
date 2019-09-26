<template>
<div v-if="loadbox">
    <div v-if="nodate" class="recommend-content">
        <div>
            <div class="module">
                <ul class="clearfix">
                    <li><span class="fw">WWL NO：</span> {{hbl.workNo}}</li>
                    <li><span class="fw">H/B L：</span> {{hbl.hblNo}}</li>
                    <li><span class="fw">{{$t('common.shipCom')}}：</span> {{hbl.shippingCode}}</li>
                    <li><span class="fw">{{$t('common.Carrier')}}：</span> {{hbl.vessel}}</li>
                    <li><span class="fw">{{$t('common.Vessel')}}：</span> {{hbl.voyage}}</li>
                    <li><span class="fw">{{$t('common.PlaceOfReceipt')}}：</span> {{hbl['placeReceiptEn']}}</li>
                    <li><span class="fw">{{$t('common.POL')}}：</span> {{hbl['portStartEn']}}</li>
                    <li><span class="fw">{{$t('common.POD')}}：</span> {{hbl['portEndEn']}}</li>
                    <li><span class="fw">{{$t('common.PlaceofDelivery')}}：</span> {{hbl['placeReceiptEn']}}</li>
                    <li><span class="fw">ETD：</span> {{hbl.etd | dateInit('MM-DD-YYYY',language)}}</li>
                    <li><span class="fw">{{$t('common.Paymentmethod')}}：</span> {{hbl['payMethod'+language]}}</li>
                    <li><span class="fw">{{$t('common.Signingmethod')}}：</span> {{hbl['blMenthod'+language]}}</li>
                    <li><span class="fw">{{$t('common.Transportation')}}：</span> {{hbl['transportClause'+language]}}</li>
                </ul>
            </div>

            <div class="module">
                <p class="border-bottom tit-pic">
                    <i class="iconfont icon-hezuo"></i>
                    {{$t('common.Parties')}}
                </p>
                <ul class="clearfix">
                    <li class="tit">{{$t('order.Consigner')}}:</li>
                    <!-- <li>{{$t('common.Name')}}： {{hbl.fName}}</li> -->
                </ul>
                <div class="text-area bor-b">
                    <div class="textarea">
                        {{hbl.fAddress}}
                    </div>
                </div>
                <ul class="clearfix">
                    <li class="tit">{{$t('order.Consignee')}}： </li>
                    <!-- <li>{{$t('common.Name')}}： {{hbl.sName}}</li> -->
                </ul>
                <div class="text-area bor-b">
                    <div class="textarea">
                        {{hbl.sAddress}}
                    </div>
                </div>
                <ul class="clearfix">
                    <li class="tit">{{$t('common.Notify')}}： </li>
                    <!-- <li>{{$t('common.Name')}}： {{hbl.tName}}</li> -->
                </ul>
                <div class="text-area bor-b">
                    <div class="textarea">
                        {{hbl.tAddress}}
                    </div>
                </div>
                <!-- <ul class="clearfix">
                    <li class="tit">{{$t('common.Secondnotify')}}： </li>
                    <li>{{$t('common.Name')}}： {{hbl.tNameSecond}}</li>
                </ul>
                <div class="text-area bor-b">
                    <div class="textarea">
                        {{hbl.tAddressSecond}}
                    </div>
                </div> -->
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
                                    <!-- <th width="100">{{$t('common.SOC')}}</th>
                                    <th width="100">{{$t('common.remark')}}</th> -->
                                    <th width="100">{{$t('common.jianshu')}}</th>
                                    <th width="100">{{$t('common.Packing')}}</th>
                                    <th width="100">{{$t('common.Weight')}}</th>
                                    <th>{{$t('common.size')}}</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="(item,index) in hblNumList" :key="index">
                                    <td>{{item['ctnType'+language]}}</td>
                                    <td>{{item.number}}</td>
                                    <td>{{item.ctnNo}}</td>
                                    <td>{{item.sealNo}}</td>
                                    <!-- <td>{{item['socFlag'+language]}}</td>
                                    <td>{{item.remark}}</td> -->
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
                    <i class="iconfont icon-dingdan"></i>
                    {{$t('common.CargoInformation')}}
                </p>
                <ul class="clearfix">
                    <li class="tit">{{$t('common.Shippingmark')}}： </li>
                </ul>
                <div class="text-area bor-b">
                    <div class="textarea">
                        {{hblGoods.marks}}
                    </div>
                </div>
                <ul class="clearfix">
                    <li class="tit">{{$t('common.Tradename')}}： </li>
                </ul>
                <div class="text-area bor-b">
                    <div class="textarea">
                        {{hblGoods.goods}}
                    </div>
                </div>
                <ul class="clearfix">
                    <li class="tit">{{$t('common.remark')}}： </li>
                </ul>
                <div class="text-area bor-b">
                    <div class="textarea">
                        {{hblGoods.remark}}
                    </div>
                </div>
                <ul class="clearfix">
                    <li><span class="fw">{{$t('order.goodsType')}}：</span> {{hblGoods.goodsTypeNameCn}}</li>
                    <li><span class="fw">{{$t('order.Number')}}：</span> {{hblGoods.number}}  </li>
                    <li><span class="fw">{{$t('order.packtype')}}：</span> {{hblGoods['packageType'+language]}}</li>
                    <li><span class="fw">{{$t('order.weigth')}}：</span> {{hblGoods.weight}}KGS</li>
                    <li><span class="fw">{{$t('order.Volume')}}：</span> {{hblGoods.volume}}CBM</li>
                    <li><span class="fw">HS CODE：</span> {{hblGoods.hsCode}}</li>
                    <div v-if="hblGoods.goodsType==2">
                        <li><span class="fw">{{$t('order.wxlb')}}：</span> {{hblGoods.dgType}}</li>
                        <li><span class="fw">{{$t('order.UNNumber')}}：</span> {{hblGoods.unNo}}</li>
                        <li><span class="fw">{{$t('order.wxym')}}：</span> {{hblGoods.dgPageNo}}</li>
                        <li><span class="fw">{{$t('order.wpsd')}}：</span> {{hblGoods.flashPoint}}</li>
                        <li><span class="fw">PACKING GROUP：</span> {{hblGoods.packingGroup}}</li>
                    </div>
                    <div v-if="hblGoods.goodsType==3">
                        <li><span class="fw">{{$t('order.Temperature')}}：</span> {{hblGoods.temperature}}</li>
                        <li><span class="fw">{{$t('order.Ventilation')}}：</span> {{hblGoods.ventilation}}</li>
                        <li><span class="fw">{{$t('order.Luggage')}}：</span> {{hblGoods.containerStuffingDate}}</li>
                    </div>
                    <div v-if="hblGoods.goodsType==4">
                        <li><span class="fw">{{$t('order.Length')}}</span>： {{hblGoods.length}}CM</li>
                        <li><span class="fw">{{$t('order.Width')}}</span>： {{hblGoods.width}}CM</li>
                        <li><span class="fw">{{$t('order.Height')}}</span>： {{hblGoods.height}}CM</li>
                    </div>
                </ul>
            </div>

            <!-- <div class="module">
                <p class="border-bottom tit-pic">
                    <i class="iconfont icon-dingdan"></i>
                    {{$t('common.Documentaryinfo')}}
                </p>
                <ul class="clearfix">
                    <li>{{$t('common.Documentary')}}： {{hbl.contract}}</li>
                    <li>{{$t('order.tel')}}： {{hbl.tel}}</li>
                    <li>{{$t('setting.email')}}： {{hbl.email}}</li>
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
    // import {toBookingDetailHbl} from '@/api/getData'
    import { toDetailBooking } from '@/api/sysapi';
    import {mapState} from 'vuex'
    export default {
        mounted(){
            if(this.nodate){
               
            }
        },
        props:['params'],
        data(){
            return {
                hbl:{},
                hblGoods:{},
                hblNumList:[],
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
                    scrollY:true,
                })
            },
            init(){
                let {id} = this.params;
                //  let {id} = this.$route.query
                toDetailBooking('toDetailBookingHbl',{bookingId:id}).then(res=>{
                    let {hbl,hblGoods,hblNumList} = res.data;
                    this.hbl = hbl||{};
                    this.hblGoods = hblGoods||{};
                    this.hblNumList = hblNumList||[];
                    this.loadbox = true;
                    if(!Object.keys(this.hbl).length&&!Object.keys(this.hblGoods).length&&!this.hblNumList.length){
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
            display: flex; align-items: center;  font-size: .28rem; color: #3291ef; box-sizing: border-box; padding: 5px 0; line-height: 25px; margin-bottom: 5px;
            i{
                font-size: .3rem; color:  #3291ef; margin-right: 8px;
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