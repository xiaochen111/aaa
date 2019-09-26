<template>
    <div class="fcl-detail">
        <van-nav-bar
            title="运价详情"
            :left-text="$t('common.goback')"
            left-arrow
            fixed
            @click-left="onClickLeft"
        />

        <scroll ref="scroll" class="recommend-content">
            <div class="shoot-t">
                <div class="shoot-screen">

                    <div class="main-top">
                        <!-- <p class="price"><span>${{this.totalPrice.totalUsd}}</span><span>￥{{this.totalPrice.totalCny}}</span></p> -->
                        <!-- <p class="price"><span>${{meiall}}</span><span>￥{{RMB}}</span></p> -->
                        <div class="pic-intro">
                            <div class="shippingIdCode">
                                <img :src="imgrul||itemDetail.shippingIdOther">
                                <p>{{itemDetail.shippingIdCode}}</p>
                            </div>
                            <div class="detail-contxt">
                                <p class="tit-h">{{itemDetail.portStartIdNameEn}}-{{itemDetail.portEndIdNameEn}}</p>
                                <!-- <p>{{itemDetail.wharfIdNameCn}}&nbsp;&nbsp;{{$t('common.week')}}{{itemDetail.schedule}}&nbsp;&nbsp;{{itemDetail.transFlag===1?$t('common.Direct'):$t('common.Trans')}}&nbsp;&nbsp;{{itemDetail.voyage}}{{$t('common.day')}}&nbsp;&nbsp;{{itemDetail.beginDate | dateInit}}~{{itemDetail.endDate | dateInit}}</p> -->
                                <p>
                                    <span v-if="language=='Cn'">{{$t('common.Schedule')}}：{{itemDetail.schedule|weekInitCn}}</span>
                                    <span v-else>{{$t('common.Schedule')}}：{{itemDetail.schedule|weekInitEn}}</span>
                                </p>
                                <p>
                                    <span>{{itemDetail['wharfIdName'+language]}}</span>
                                    <span>{{itemDetail.voyage}}{{$t('common.day')}}</span>
                                    <span>{{itemDetail.transFlag===1?$t('common.Direct'):$t('common.Trans')}}</span>
                                </p>
                                <p>{{$t('common.validity')}}:{{itemDetail.beginDate | dateInit('MM-DD-YYYY',language)}}~{{itemDetail.endDate | dateInit('MM-DD-YYYY',language)}}</p>
                                <p> 
                                    <span class="price-span" v-if="!itemDetail.price20">/</span>
                                    <span class="price-span" v-else>{{itemDetail.price20 == 999999999?$t('common.call'):itemDetail.price20}}</span>

                                    <span class="price-span" v-if="!itemDetail.price40">/</span>
                                    <span class="price-span" v-else>{{itemDetail.price40 == 999999999?$t('common.call'):itemDetail.price40}}</span>

                                    <span class="price-span" v-if="!itemDetail.price40hq">/</span>
                                    <span class="price-span" v-else>{{itemDetail.price40hq == 999999999?$t('common.call'):itemDetail.price40hq}}</span>
                                </p>
                                
                            </div>
                        </div>
                    </div>

                    <div class="list-li" v-show="surChageList.length">
                        <ul>
                            <van-cell-swipe :right-width="65"  v-for="(item,index) in surChageList" :key="index">
                                
                                <li class="border-bottom">
                                    <div class="main-left">
                                        
                                        <img :src="item.currency==1?'./static/images/icon/meiyuan.png':'./static/images/icon/renminbi.png'" alt="" srcset="">
                                        <p class="tit">
                                            <span v-if="language=='Cn'">{{item.chargeName}}</span>
                                            <span v-else>{{item.chargeNameEn}}</span>
                                            <span v-if="item.unit==1">{{item.billPrice}}</span>
                                            <span v-else-if="item.unit==2">{{item.price20}}/{{item.price40}}/{{item.price40hq}}</span>
                                        </p>
                                    </div>
                                    <div class="main-rigth" v-if="item.unit==1">{{item.billPrice}} {{item.currency==1?'USD':'CNY'}}</div>
                                    <div class="main-rigth" v-else-if="item.unit==2">{{(item.price20||0)*value.price20Num + (item.price40||0)*value.price40Num + (item.price40hq||0)*value.price40HqNum }} {{item.currency==1?'USD':'CNY'}} </div>
                                </li>
                                <span slot="right"  class="delete" @click="deleteitem(index)">{{$t('common.delete')}}</span>
                            </van-cell-swipe>
                        </ul>
                    </div>
                    
                    <div class="module" v-if="itemDetail.descWeight">
                        <p class="tit border-bottom">{{$t('common.Weightlimit')}}</p>
                        <div class="module-content">
                            {{itemDetail.descWeight}}
                        </div>
                    </div>
                    <div class="module" v-if="itemDetail.remarkIn">
                        <p class="tit border-bottom">{{$t('common.remark')}}</p>
                        <div class="module-content">
                            {{itemDetail.remarkIn}}
                        </div>
                    </div>
                </div>
            </div>
        </scroll>

        <ul class="tab-bot">
            <li @click="saveImg">
                <i class="iconfont icon-shengchengbaojiadan"></i>
                {{$t('common.scbjd')}}
            </li>
            <li @click="additional"> 
                <i class="iconfont icon-shaixuan"></i>
                {{$t('common.addcost')}}
            </li>
            
        </ul>

        <van-popup v-model="showImg">
            <img :src="imgUrl" alt="" srcset="" width="100%">
            <p class="imgsave">{{$t('common.longpress')}}</p>
        </van-popup>


      

        <span class="load-box" v-if="loadshow">
            <van-loading type="spinner" color="black" />
        </span>
    </div>
</template>

<script>
    import { NavBar,Tabbar,TabbarItem,Stepper,Popup,Loading,CellSwipe } from 'vant';
    import Scroll from '../../../common/scroll';
    import html2canvas from 'html2canvas';
    import {mapState} from 'vuex'
    import telbox  from '@/components/common/tel'
    import bus from '@/utils/eventBus'
    import {permissionsDef} from '@/utils/common'
    // import {getFreightFclInfo,querySurchargeFcl2Json,saveFclQuote,wwlconsult,getImg} from '../../../../api/getData';
    import { getFreightFclInfo ,getSurchargeList,getImg} from '@/api/sysapi';
    export default {
        components:{
            [NavBar.name]:NavBar,
            [Tabbar.name]:Tabbar,
            [TabbarItem.name]:TabbarItem,
            [CellSwipe.name]:CellSwipe,
            [Stepper.name]:Stepper,
            [Popup.name]:Popup,
            [Loading.name]:Loading,
            Scroll,
            telbox,
        },
        data(){
            return {
                itemDetail:{},
                value:{
                    price20Num:1,
                    price40Num:0,
                    price40HqNum:0,
                },
                surChageList:[],
                totalPrice:{
                    totalCny:0,
                    totalUsd:0,
                },
                showImg:false,
                imgUrl:'',
                loadshow:false,

                meiall:0,
                RMB:0,
                fujiafeiAll:0,

                telshow:false,

                imgrul:'',

            }
        },
        created() {
            console.log(this.$route)
            let {freightFclId,freightLevelId} = this.$route.query
            getFreightFclInfo({freightFclId,freightLevelId}).then(res=>{
                this.itemDetail = res.data;
            })
            getSurchargeList({freightId:freightFclId}).then(res=>{
                this.surChageList = res.data
                if(this.surchargehttpFCLArr.flag){
                    this.surChageList = this.surchargeFCLArr?this.surchargehttpFCLArr.arr.concat(this.surchargeFCLArr):this.surchargehttpFCLArr.arr;
                    return
                }
            })
        },
        methods:{
            onClickLeft(){
                history.go(-1);
            },
            additional(){
                this.$router.push('/additional')
                this.surchargehttpFCLArr.arr = this.surChageList;
            },
            calculate(){ //费用计算

                let unitArrRMB = this.surChageList.filter(item => { //人民币的集合
                    return item.currency==2
                })

                let RMB = 0;
                unitArrRMB.forEach(item => {
                    if(item.unit == 1){
                        RMB+=item.billPrice*1
                    }else if(item.unit == 2){
                        let {price20,price40,price40hq} = item;
                        let {price20Num,price40Num,price40HqNum} = this.value
                        RMB+=(price20||0)*price20Num*1 + (price40||0)*price40Num*1 + (price40hq||0)*price40HqNum*1
                    }
                });
                this.RMB = RMB


                if(this.itemDetail.price20 == 999999999) return;


                let unitArr = this.surChageList.filter(item => { //美元的集合
                    return item.currency==1
                })

                let fuMei = 0;
                unitArr.forEach(item => {
                    if(item.unit == 1){
                        fuMei+=item.billPrice*1
                    }else if(item.unit == 2){
                        let {price20,price40,price40hq} = item;
                        let {price20Num,price40Num,price40HqNum} = this.value
                        fuMei+=(price20||0)*price20Num*1 + (price40||0)*price40Num*1 + (price40hq||0)*price40HqNum*1
                    }
                });
                
                let {price20,price40,price40hq} = this.itemDetail;
                let {price20Num,price40Num,price40HqNum} = this.value
                this.meiall = (price20||0)*price20Num*1 + (price40||0)*price40Num*1 + (price40hq||0)*price40HqNum*1 + fuMei;
                
            },
            calculateFn(target){
                if(this.surchargehttpFCLArr.flag){
                    this.surChageList = this.surchargeFCLArr?this.surchargehttpFCLArr.arr.concat(this.surchargeFCLArr):this.surchargehttpFCLArr.arr;
                    this.calculate();
                    return
                }
                querySurchargeFcl2Json(this.$route.query).then(res=>{
                    this.surChageList = res.data
                    this.calculate();
                })
            },
            saveImg(){
                this.loadshow = true;
                getImg({imgUrl:this.itemDetail.shippingIdOther}).then(res=>{
                        console.log(res.data)
                        this.imgrul = 'data:image/png;base64,'+ res.data;
                        html2canvas(document.querySelector(".shoot-screen")).then(canvas => {
                            this.loadshow = false;
                            var dataUrl  = canvas.toDataURL("image/png");
                            this.imgUrl = dataUrl;
                            this.showImg = true;
                            // saveFclQuote(this.$route.query)
                    });
                })
            },
            deleteitem(index){
                this.surChageList.splice(index,1)
                this.calculate()
            },
            wwlconsultFn(){
                bus.$emit('telshow',true)
                wwlconsult()
            },
            wwlconsultFn1(){
                wwlconsult()
            },
        },
        computed:{
            ...mapState(['language','tel','surchargeFCLArr','surchargehttpFCLArr','loginflag']), 
        },
    }
</script>

<style lang="less" scoped>
    .fcl-detail{
        padding-top: 46px; padding-bottom: .9rem;  overflow: hidden; height: calc(~'100% - .9rem - 46px');
    }
    .imgsave{
             font-size: .22rem; color: #333; text-align: center;
    }
    .module{
        box-sizing: border-box; padding-left: .2rem; padding-bottom: .1rem; margin-top: .1rem; background: #fff;
        .tit{
            line-height: .8rem; font-size: .28rem; color: #333;
        }
        .module-content{
            font-size: .24rem; color: #666; line-height: .35rem; box-sizing: border-box; padding-top: .1rem;
        }
    }
    .main-top{
        box-sizing: border-box; padding: .3rem .8rem; background: #fff;
        .price{
            display: flex; justify-content: space-between; box-sizing: border-box; padding: 0 .3rem;
            font-size: .34rem; color: #f36363; 
        }
        .pic-intro{
            display: flex;margin: .2rem 0; align-items:flex-start;
            .shippingIdCode{
                font-size: .2rem;text-align: center; overflow: hidden; width: 1rem;
            }
            img{
                width: .6rem; height: .6rem;
            }
            .detail-contxt{
                display: flex; flex-direction: column; box-sizing: border-box; padding-left: .4rem; width: 95%;
                p{
                    font-size: .2rem; color: #666; margin-bottom: .1rem; display: flex; justify-content: space-between;
                    .price-span{
                        display: inline-block; width: .6rem;font-size: .26rem; color: #f36363;
                    }
                }
                .tit-h{
                    font-size: .26rem; color: #333;
                }
            }
        }
        .step{
            display: flex; justify-content: center; margin-bottom: .1rem;
            span{
                font-size: .2rem; color: #666; margin-right: .2rem; align-items: center; line-height: 30px;
            }
        }
    }
    .list-li{
        box-sizing: border-box; background: #fff; margin: .1rem 0;
        ul{
            li{
                display: flex; justify-content: space-between; align-items: center; padding: .15rem .2rem;
            }
        }

        .main-left{
           display: flex;  align-items: center;
           img{
               width: .48rem;
           }
        }
        .tit{
            display: flex; flex-direction: column; margin-left: .2rem;
            span:first-child{
                font-size:.26rem; color: #666;
            }
            span:nth-child(2){
                font-size:.18rem; color: #959595;
            }
        }
        .main-rigth{
            font-size: .3rem; color: #f36363;
        }
        .mark{
            font-size: .18rem; color: #959595; height: .7rem; display: flex; align-items: center; padding-left: .2rem;
        }
    }
    .tab-bot{
        height: .9rem; background: #fff; position: fixed; bottom: 0; width: 100%;
        display: flex; align-items: center; justify-content: space-around;box-shadow: 0 -10px 13px -10px rgba(0,0,0,0.35);
        li,a{
            font-size: .2rem; color: #333;  text-align: center; display: flex; 
            align-items: center; flex-direction: column;
            img{
                width: .4rem; height: .4rem;
            }
        }
        i{
            font-size: .45rem;
        }
    }
    .delete{
        height: 100%; width: 65px; background: #3291ef; color: #fff; text-align: center;  line-height: 4; display: inline-block;
        font-size: .22rem;vertical-align: top;
    }
</style>