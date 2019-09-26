<template>
<div v-if="loadbox">
    <div v-if="nodate" class="recommend-content">
        <div>
            <div class="module">
                <ul class="clearfix">
                    <li>M/B L： {{vgm.mblNo}}</li>
                    <li>{{$t('common.shipCom')}}： {{vgm.shippingCode}}</li>
                    <li>{{$t('order.cmhc')}}： {{vgm.vessel}}</li>
                    <li>{{$t('common.port')}}： {{vgm['portStart'+language]}}</li>
                    <li>{{$t('common.Authorized')}}： {{vgm.authorizer}}</li>
                    <li>{{$t('common.Authorized')}} E-mail： {{vgm.authorizerEmail}}</li>
                    <li>{{$t('common.Shipper')}}： {{vgm.shipper}}</li>
                    <li>{{$t('common.Responsibleparty')}}： {{vgm.responsibleParty}}</li>
                    <li>{{$t('common.ShipperAddress')}}： {{vgm.shipperAddress}}</li>
                    <li>{{$t('common.Respartyaddr')}}： {{vgm.responsiblePartyAddress}}</li>
                </ul>
            </div>

            <div class="module">
                <div class="tit-table">{{$t('common.typevolume')}}</div>
                <div class="content" ref="content">
                    <div class="long">
                        <table border="1" >
                            <thead>
                                <tr>
                                    <th width="100">{{$t('common.BoxNo')}}</th>
                                    <th width="100">{{$t('common.Sealno')}}</th>
                                    <th width="100">{{$t('common.Boxtypesize')}}</th>
                                    <th width="100">{{$t('common.Boxclass')}}</th>
                                    <th width="100">VGM</th>
                                    <th width="100">{{$t('common.unit')}}</th>
                                    <th width="100">{{$t('common.Weighmethod')}}</th>
                                    <th width="100">{{$t('common.Weightime')}}</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="(it,index) in vgmConList" :key="index">
                                    <td>{{it.ctnNo}}</td>
                                    <td>{{it.sealNo}}</td>
                                    <td>{{it['ctnType'+language]}}</td>
                                    <td>{{it['ctnLei'+language]}}</td>
                                    <td>{{it.vgms}}</td>
                                    <td>{{it.unit}}</td>
                                    <td>{{it['weighingMethod'+language]}}</td>
                                    <td>{{it.weighingTime | dateInit('MM-DD-YYYY',language)}}</td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
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
    import {toBookingDetailVgm} from '@/api/getData';
    import {mapState} from 'vuex'
    export default {
        data(){
            return {
                vgm:{},
                vgmConList:[],
                nodate:true,
                loadbox:false,
            }
        },
        props:['params'],
        created() {
            this.getToBookingDetailVgm();
        },
        computed:{
            ...mapState(['language']),  //这个是后台中英文翻译的关键
        },
        methods:{
            initScrollX(){ //better-scroll 横线滚动条
                let contentX = document.querySelectorAll('.content')
                for(let i = 0; i< contentX.length; i++){
                    this['scroll'+i]=new BScroll(contentX[i], {
                        startX:0,
                        click:true,
                        scrollX:true,
                        scrollY:false,
                    })
                }
            },
            getToBookingDetailVgm(){
                let {id} = this.params;
                toBookingDetailVgm({bookingId:id}).then(res=>{
                    let {vgm,vgmConList} = res.data;
                    this.vgm = vgm||{};
                    this.vgmConList = vgmConList||[];
                     this.loadbox = true
                    if(!Object.keys(this.vgm).length&&!this.vgmConList.length){
                        this.nodate = false
                    }else{
                        this.nodate = true
                        setTimeout(()=>{
                            this.scrollDef();
                            this.wrapper.refresh();
                        },20)
                    }
                })
            },
            scrollDef(){
                 // 横向
                this.initScrollX();
                // 纵向
                let wrapper = document.querySelector('.recommend-content');
                this.wrapper = new BScroll(wrapper)
            }
        },
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
                &.col-6{
                    width: 50%; float: left;
                }
                .font-red{
                    color: #f42434;
                }
            }
        }
        .tit-pic{
            display: flex; align-items: center;  font-size: .28rem; color: #3291ef;box-sizing: border-box; padding: 5px 0; line-height: 25px; margin-bottom: 5px;
            img{
                width: 20px; margin-right: 5px;
            }
            i{
                font-size: .3rem; color: #3291ef; margin-right: 8px;
            }
        }
        .text-area{
            &.bor-t{
                box-sizing: border-box; padding:.15rem 0; border-top: 1px dashed #ccc;
            }
            &.bor-b{
                box-sizing: border-box; padding:.15rem 0; border-bottom: 1px dashed #ccc; margin-bottom: .1rem;
            }
            textarea{
                font-size: .24rem; outline: none; border: 1px solid #cdcccc; width: 100%;border-radius: 4px; box-sizing: border-box; padding: 5px; display: block;
                background: #eee;
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