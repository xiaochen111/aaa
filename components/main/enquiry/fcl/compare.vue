<template>
    <div id="compare">
        <van-nav-bar :title="$t('common.contrast')" :left-text="$t('common.goback')" left-arrow fixed  @click-left="onClickLeft">
        </van-nav-bar>

        <scroll  ref="scroll" class="recommend-content" :data="list">
            <div class="main-all">

                <div class="tit-left">
                    <ul>
                        <li>{{$t('common.typevolume')}}</li>
                        <li>{{$t('common.shipCom')}}</li>
                        <li>{{$t('common.voyage')}}</li>
                        <li>{{$t('common.Trans')}}/{{$t('common.Direct')}}</li>
                        <li>{{$t('common.Schedule')}}</li>
                        <li>{{$t('common.freightprice')}}</li>
                        <li :style="styleObj">{{$t('common.Surcharge')}}</li>
                        <li class="allPrice">{{$t('common.Total')}}</li>
                    </ul>
                </div>
                <div class="content" ref="content">
                    <div class="main-content">
                        <div class="calc-x">
                            <div class="step"><span>20GP</span><van-stepper :min="0" v-model="value.price20Num" @change="listInit"/></div>
                            <div class="step"><span>40GP</span><van-stepper :min="0" :default-value="0" v-model="value.price40Num" @change="listInit"/></div>
                            <div class="step"><span>40HQ</span><van-stepper :min="0" :default-value="0" v-model="value.price40HqNum" @change="listInit" /></div>
                        </div>
                        <div class="listAll">
                            <ul v-for="(item,index) in list" :key="index" :style="{width: list.length>2?'2.6rem':'3.92rem'}">
                                <li>{{item.shippingIdCode}}</li>
                                <li>{{item.voyage}}{{$t('common.day')}}</li>
                                <li>{{item.transFlag===1?$t('common.Direct'):$t('common.Trans')}}</li>
                                <li>
                                    <span v-if="language=='Cn'">{{item.schedule|weekInitCn}}</span>
                                    <span v-else>{{item.schedule|weekInitEn}}</span>
                                </li>
                                <li class="yj" v-if="priceFclShow">
                                    <span>{{item.price20 == 999999999?$t('common.call'):item.price20}}</span>
                                    <span>{{item.price40 == 999999999?$t('common.call'):item.price40}}</span>
                                    <span>{{item.price40hq == 999999999?$t('common.call'):item.price40hq}}</span>
                                </li>
                                <li class="yj" v-else>
                                    <span>——</span>
                                    <span>——</span>
                                    <span>——</span>
                                </li>
                                <li class="surcharge" :style="styleObj">
                                    <dl>
                                        <dd class="border-bottom" v-for="(itemcharge,index) in item.surChageList" :key="index">
                                            <p v-if="language=='Cn'" class="tit">{{itemcharge.chargeName}}</p>
                                            <p v-else class="tit">{{itemcharge.chargeNameEn}}</p>
                                            <p class="dj" v-if="itemcharge.unit==1">{{itemcharge.billPrice}}</p>
                                            <p class="dj" v-else-if="itemcharge.unit==2">{{itemcharge.price20}}/{{itemcharge.price40}}/{{itemcharge.price40hq}}</p>
                                            <p class="tit">{{itemcharge.totalPrice}} {{itemcharge.currency==1?'USD':'CNY'}}</p>
                                        </dd>
                                    </dl>
                                </li>
                                <li class="allPrice" v-if="priceFclShow">${{item.totalUsd}}&nbsp;&nbsp;&nbsp;￥{{item.totalCny}}</li>
                                <li class="allPrice" v-else>——&nbsp;&nbsp;&nbsp;——</li>
                            </ul>
                        </div>
                    </div>
                </div>
            </div>
        </scroll>
    </div>
</template>

<script>
    import { NavBar,Stepper } from 'vant';
    import Scroll from '../../../common/scroll';
    import BScroll from 'better-scroll'
    import {mapState,mapMutations} from 'vuex'
    import {queryBiJiaDetailList} from '../../../../api/getData';
    import {permissionsDef} from '@/utils/common'
    
    export default {
        components:{
            [NavBar.name]:NavBar,
            [Stepper.name]:Stepper,
            Scroll,
        },
        data(){
            return {
                list:[],
                value:{
                    price20Num:1,
                    price40Num:0,
                    price40HqNum:0,
                },
                maxlen:0,
                styleObj:{
                    height:'auto'
                },
                priceFclShow:true,// 权限判断 是否显示运价
            }
        },
        mounted() {
            this.initScrollX();

            this.listInit()
        },
        created() {
             // 权限判断 是否显示运价
            this.priceFclShow = this.priceFclShowBoolean();
        },
        methods:{
            onClickLeft(){
                history.go(-1);
            },
            initScrollX(){ //better-scroll 横线滚动条
                if (!this.scroll){
                    this.scroll=new BScroll(this.$refs.content, {
                        startX:0,
                        click:true,
                        scrollX:true,
                        scrollY:false,
                        eventPassthrough:'vertical'
                    })
                }else{
                    this.scroll.refresh()
                }
            },

            listInit(){
                // let paramsStr = this.compareFcl.join();
                // let target = {freightIds:paramsStr}
                let target =this.$route.query;
                Object.assign(target,this.value)
                queryBiJiaDetailList(target).then(res=>{
                    res.data.forEach(item => {
                        if(item.surChageList.length>this.maxlen){
                            this.maxlen = item.surChageList.length
                        }
                    });
                    if(this.maxlen){
                        this.styleObj.height = this.maxlen*1.2+0.2+'rem'
                    }

                    this.list = res.data;
                })
            },

             // 权限判断 是否显示运价
            priceFclShowBoolean(){
                let showForFob = permissionsDef('/web/freight/freightfclweb/queryFreightFclListForFob.html');
                let showForPlat = permissionsDef('/web/freight/freightfclweb/queryFreightFclListForPlat.html');
                return showForFob || showForPlat
            }
        },
        computed:{
            ...mapState(['language','compareFcl','loginflag']), 
        },
    }
</script>

<style lang="less" scoped>
    #compare{
        box-sizing: border-box; padding-top: 46px; height: 100%; overflow: hidden; color: #676a6c; 
        .main-all{
            position: relative;
        }
        .tit-left{
            position: absolute; left: 0 ; top: 0;width: 1.4rem; font-size: .22rem; z-index: 9;  border-right: 1px solid #cdcccc;background: #fff;//height: 1300px;
            ul{
                li{
                    height: .85rem; line-height: .85rem;text-align: center; font-size: .22rem;border-bottom: 1px solid #cdcccc;  overflow: hidden;text-overflow: ellipsis; white-space: nowrap; 
                }
            }
        }
        .main-content{
            box-sizing: border-box; padding-left: 1.4rem; width: 9.25rem; background: #fff;//height: 1300px; 
            .calc-x{
                border-bottom: 1px solid #cdcccc;height: .85rem; line-height: .85rem; font-size: .22rem;justify-content: space-around; display: flex;
                align-items: center;
                .step{
                    display: flex;  margin-bottom: .1rem; align-items: center;
                    span{
                        font-size: .2rem; color: #666; margin-right: .2rem; align-items: center; line-height: 30px;
                    }
                }
            }
            .listAll{
                display: flex; justify-content: flex-start;
            }
            ul{
                width: 2.6rem;border-right: 1px solid #cdcccc;
                li{
                    min-height: .85rem; line-height: .85rem;text-align: center; font-size: .22rem;border-bottom: 1px solid #cdcccc;
                }
                .yj{
                    display: flex; justify-content: space-around; color: #fe624d;
                }
                // .surcharge{
                //     box-sizing: border-box; padding: 10px 5px;
                // }
                dl{                          
                    padding: .1rem .05rem; width: 90%; margin-left: 5%;
                    dd{
                        padding-bottom: .1rem; box-sizing: border-box;
                        p{
                            text-align: left;box-sizing: border-box; padding-left: .2rem;
                        }
                        .tit{
                            font-size: .22rem; line-height: .4rem; height: .4rem;
                        }
                        .dj{
                            font-size: .18rem; line-height: .2rem;height: .2rem;margin-bottom: .1rem;
                        }
                    }
                }
            }
            // .box1{
            //     width: 1rem; height: 1rem;border-right: 1px solid #cdcccc;
            // }
            // .box2{
            //     width: 1rem; height: 3rem;border-right: 1px solid #cdcccc;
            // }
            // .box3{
            //     width: 1rem; height: 2rem;border-right: 1px solid #cdcccc;
            // }
        }
        .box{
            width: 100%; height: 1.2rem;border-right: 1px solid #cdcccc;
        }
        .allPrice{
            font-size: .26rem; color: #f42434;
        }


        // table{
        //     table-layout: fixed; background: #fff; width: 8.25rem;border-collapse:collapse; 
        //     td{
        //         border:1px solid #cdcccc; font-size: .22rem; padding: .15rem .1rem;margin-top: -1px; margin-left: -1px;
        //     }
        //     tr{
        //          line-height: 30px;
        //     }
        //     .t-title{
        //         width: 1.3rem;
        //     }
        //     .t-item{
        //         width: 2.3rem;
        //     }
        // }
    }
</style>