<template>
<div class="adviceDetail">
    <nav-bar :title="$t('common.advice')"></nav-bar>

    <div v-if="nodate" class="recommend-content">
        <div>
            <div class="module">
                <p class="border-bottom tit-pic">
                   <span>
                        <i class="iconfont icon-shengchengbaojiadan"></i>
                         M/B L
                   </span>
                </p>
                <ul v-for="(item,index) in mblList" :key="index">
                    <li>M/B L: {{item.mblNo}}</li>
                    <li>{{$t('common.Carrier')}}/{{$t('common.Vessel')}}: {{item.vessel}}/{{item.voyage}}</li>
                    <li class="border">
                        <span>ETD: {{item.etd | dateInit('MM-DD-YYYY',language) }}</span>
                        <a :href="item.filePath"><i class="iconfont icon-xiazai"></i></a>
                    </li>
                </ul>
            </div>

            <div class="module">
                <p class="border-bottom tit-pic">
                   <span>
                        <i class="iconfont icon-shengchengbaojiadan"></i>
                        H/B L
                   </span>
                </p>
                <ul v-for="(item,index) in hblList" :key="index">
                    <li>M/B L: {{item.hblNo}}</li>
                    <li>{{$t('common.Carrier')}}/{{$t('common.Vessel')}}: {{item.vessel}}/{{item.voyage}}</li>
                    <li class="border">
                        <span>ETD: {{item.etd | dateInit('MM-DD-YYYY',language) }}</span>
                        <a :href="item.filePath"><i class="iconfont icon-xiazai"></i></a>
                    </li>
                </ul>
            </div>
 
            <div class="module">
                <p class="border-bottom tit-pic">
                    <span>
                        <i class="iconfont icon-dingdan"></i>
                        {{$t('common.Accountstatus')}}：{{account['status'+language]}}
                    </span>
                    <span>{{$t('common.ordertype')}}：{{account['accountType'+language]}}</span>
                </p>
                <ul class="clearfix">
                    <li>{{$t('common.accountNo')}}： {{account.accountNo}}</li>
                    <li>{{$t('common.SettlementCompany')}}： {{account.invoiceCompany}}</li>
                    <li>{{$t('common.Invoicetitle')}}： {{account.invoiceHead}}</li>
                    <li>{{$t('common.billType')}}： {{account['shouzhi'+language]}}</li>
                    <li>{{$t('common.Settlementcustomer')}}： {{account['settlement'+language]}}</li>
                    <li>{{$t('common.Settlementcountry')}}： {{account.settlementCountry}}</li>
                    <li>{{$t('common.Bankaccount')}}： {{account.bankNo}}</li>
                    <li>{{$t('common.Currency')}}： {{account['currency'+language]}}</li>
                    <li>{{$t('common.JobNo')}}： {{account.workNo}}</li>
                    <li>{{$t('common.Accounttype')}}： {{account.actingType}}</li>
                    <li>{{$t('common.InvoiceTime')}}： {{account.kpTime | dateInit('MM-DD-YYYY',language)}}</li>
                </ul>
                <div class="all-detail">
                    <div class="all-box">
                        <span>{{$t('common.TotalAssets')}}</span>
                        <span>USD {{all.usdAmount}}</span>
                        <span>CNY {{all.cnyAmount}}</span>
                    </div>
                </div>
            </div>


            <div class="module">
                <p class="border-bottom tit-pic">
                   <span>
                        <i class="iconfont icon-dingdan"></i>
                        {{$t('common.Accountcharges')}}
                   </span>
                </p>
                <ul v-for="(item,index) in chargeList" :key="index">
                    <li class="item-li">
                        <div class="p-t">
                            <svg class="icon" aria-hidden="true">
                                <use :xlink:href="item['estbCurrency'+language]=='CNY'?'#icon-yunjiaxiangqing-renminbi':'#icon-yunjiaxiangqing-meiyuan'"></use>
                            </svg>
                            <p>
                                <span class="first">{{item['charge'+language]}}</span>
                                <span>{{item.type}}</span>
                            </p>
                        </div>
                        <span class="right">{{item['estbCurrency'+language]}} {{item.estbAmount}}</span>
                    </li>
                </ul>
            </div>

        </div>
    </div>

    <div v-else class="noDate">
        <svg class="icon" aria-hidden="true">
            <use xlink:href="#icon-zanwushuju"></use>
        </svg>
    </div>

</div>
</template>

<script>
    import BScroll from 'better-scroll'
    import navBar from '../../../common/navBar'
    import {queryAccountDetail} from '@/api/getData';
    import {mapState} from 'vuex'
    export default {
        components:{
            navBar
        },
        data(){
            return {
                account:{},
                chargeList:[],
                hblList:[],
                mblList:[],
                all:{},
                nodate:true,
            }
        },
        created() {
            let params = this.$route.query;
            this.getQueryAccountDetail(params);
        },
        mounted(){
            if(this.nodate){
                // 纵向
                let wrapper = document.querySelector('.recommend-content');
                this.wrapper = new BScroll(wrapper,{
                    click: true,
                })
            }
        },
        computed:{
            ...mapState(['language']),  //这个是后台中英文翻译的关键
        },
        methods:{
            getQueryAccountDetail(params){
                queryAccountDetail(params).then(res=>{
                    let {account,chargeList,mblList,hblList} = res.data;
                    this.account = account||{};
                    this.chargeList = chargeList||[];
                    this.mblList = mblList||[];
                    this.hblList = hblList||[];
                    this.all = res.data;

                    if(!Object.keys(this.account).length&&!this.chargeList.length&&!this.mblList.length&&!this.hblList.length){
                        this.nodate = false
                    }else{
                        this.nodate = true
                        this.wrapper.refresh();
                    }

                })
            }
        },
    }
</script>

<style lang="less" scoped>
    .adviceDetail{
        box-sizing: border-box; padding-top: 46px;   
    }
    .module ul li.item-li{
        display: flex; justify-content: space-between; align-items: center; border-bottom: 1px dashed #cdcccc; padding: 10px 0;
        .p-t{
            display: flex; align-items: center; justify-content: flex-start;
            svg{
                width: .6rem; height: .6rem;
            }
            p{
                span{
                    display: block; margin-left: .1rem;
                    &.first{
                        font-size: .24rem; margin-bottom: .05rem;
                    }
                    &:nth-child(2){
                        font-size: .22rem;
                    }
                }
            }
        }
        .right{
            font-size: .3rem; color: #f77d7d;
        }
    }
    .recommend-content{
        position: absolute; left: 0; right: 0;  height: calc(~'100% - 46px');
    }
    .noDate{
        height: 100%; text-align: center;
        svg{
            width: 4.5rem; height: 4.5rem;
        }
    }
    .all-detail{
        display: flex; justify-content: space-between; align-items: center; font-size: .24rem; height: 35px; border-top:1px dashed #cdcccc;
        .all-box{
            span{
                margin-right: 10px;
                &:first-child{
                    font-size: .24rem; color: #333;
                }
                &:nth-child(2){
                    color: #fe624d; font-size: .28rem;
                }
                &:nth-child(3){
                    color: #f42434; font-size: .28rem;
                }
            }
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
                &.border{
                    border-bottom: 1px dashed #cdcccc; display: flex; justify-content: space-between;
                    i{
                        font-size: .3rem; color:  #3291ef;
                    }
                }
            }
        }
        .tit-pic{
            display: flex; align-items: center; justify-content: space-between;
            font-size: .28rem; color: #3291ef; box-sizing: border-box; padding: 5px 0; line-height: 25px; margin-bottom: 5px;
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