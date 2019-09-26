<template>
    <div id="platform">
        <scroll class="recommend-content">
            <div>
                <form action="/">
                    <van-search
                        v-model="value"
                        placeholder="WWL JOB NO./REF NO./MBL NO"
                        show-action
                        background="#fff"
                        @search="onSearch"
                    >
                        <div slot="action" @click="onSearch">{{$t('common.search')}}</div>
                    </van-search>
                </form>
                <!-- 箱号、提单号、环世工作号 -->
                <div class="module">
                    <!-- <h4>{{$t('home.orderoverview')}}</h4> -->
                    <!-- <scroll class="recommend-content-x"> -->
                        <ul class="top-one">
                            <li class="li-one" @click="toOrder(0)">
                                <div class="li-content">
                                    <span class="num num0">{{threeTotol}}</span>
                                    <p class="p1">{{statusStr[0]['name'+language]}}</p>
                                </div>
                            </li>
                            <li class="li-one" @click="toOrder(1)">
                                <div class="li-content">
                                <span class="num num1">{{home.BOOKING_FOUR}}</span>
                                <p class="p1">{{statusStr[1]['name'+language]}}</p>
                                </div>
                            </li>
                            <li class="li-one" @click="toOrder(2)">
                                <div class="li-content">
                                <span class="num num2">{{home.BOOKING_SIX}}</span>
                                <p class="p1">{{statusStr[2]['name'+language]}}</p>
                                </div>
                            </li>
                        </ul>
                    <!-- </scroll> -->
                </div>

                <div class="module">
                    <ul class="nav-meau">
                        <li>
                            <router-link class="img-pad"  to="/enquiry">
                                <img src="@/asset/image/index/orderc.png" alt="" srcset="">
                                <p class="p2">{{$t("home.fclnav")}}</p>
                            </router-link>
                        </li>
                        <li>
                            <router-link class="img-pad" to="/myorder">
                                <img src="@/asset/image/index/freightp.png" alt="" srcset="">
                                <p class="p2">{{$t("common.myorder")}}</p>
                            </router-link>
                        </li>
                    </ul>
                </div>

            </div>
        </scroll>
    </div>
</template>

<script>
    import Scroll from "@/components/common/scroll";
    import { Toast,Search  } from 'vant';
    import { mapState, mapMutations, mapGetters } from "vuex";
    import {permissionsDef} from '@/utils/common'
    import { homePlate } from '@/api/sysapi';
    import tipload  from '@/components/common/tipload'
    export default {
        data() {
            return {
                myorder:false,
                advice:false,
                generalnav:false,
                home:{},
                threeTotol:'',
                value:'',
                statusStr:[{nameCn:'未开航',nameEn:'PROCESSING'},{nameCn:'已开航',nameEn:'ON BOARD'},{nameCn:'已抵达',nameEn:'ARRIVAL'}],
            }
        },
        components: {
            Scroll,
            [Toast.name]:Toast,
            [Search.name]:Search,
            tipload
        },
        methods: {
            ...mapMutations(["navTabNumSet","resetCompareFcl","noKeepAlive"]),

            onSearch(){
                console.log(this.value)
                if(!this.value){
                    let tips = this.language=='Cn'?'请输入WWL JOB NO./REF NO./MBL NO查询':'Please enter WWL JOB NO./REF NO./MBL NO query';
                    Toast(tips);
                    return
                }
                this.$router.push({
                    path:'/timenode',
                    query:{params:this.value},//传参数
                })
            },
            toOrder(statusStr){
                // if(!this.myorder){
                //     let tips = this.language=='Cn'?'请联系环世客服开通查看订单权限':'Please contact World Customer Service to open the right to view orders';
                //     Toast(tips)
                //     return
                // }
                this.$router.push({
                    path:'/myorder',
                    query:{statusStr},//传参数
                })
            },
            fcllistlink(query){
                this.$router.push({
                    path:'/fclList',
                    query,//传参数
                })
            },
            tipsPosDef(pos){
                let tipPos = document.querySelector('.tipPos');
                if(tipPos&&pos.y<-20){
                    tipPos.setAttribute('class','tipPos tipPoswidth')
                }else{
                    tipPos.setAttribute('class','tipPos')
                }
            }
        }, 
        computed:{
            ...mapState(['language','loginflag']),
        },
        created() {
            this.noKeepAlive('fcllist');
            this.noKeepAlive('myorder');
            // this.noKeepAlive('message');
            this.navTabNumSet(0);
            this.resetCompareFcl();
            this.myorder = permissionsDef('/web/webbooking/booking.html');
            this.advice = permissionsDef('/web/webro/.*');
            this.generalnav = permissionsDef('dir_1003');
            homePlate().then(res=>{
                this.home = res.data;
                this.threeTotol = this.home.BOOKING_ONE+this.home.BOOKING_TWO+this.home.BOOKING_THREE+this.home.BOOKING_SEVEN
            })

            
        },
    }
</script>

<style lang="less" scoped>
    #platform{
        height:100%;overflow: hidden; box-sizing: border-box;
        form{
            margin-bottom: .2rem;
        }
    }
    .recommend-content-x{
        width: 100%; overflow: hidden;
    }
    .module{
        background: #fff; box-sizing: border-box; padding: .2rem 0 .1rem; margin-bottom: .2rem;
        h4{
            font-weight: normal; font-size: .3rem; color: #333; box-sizing: border-box; padding: 0 .2rem; margin-bottom: .1rem; text-align: center;
        }
        .top-content-one{
            display: inline-block;white-space:nowrap; height: auto;
            
        }
        ul{
            box-sizing: border-box; padding: .1rem .15rem; display: flex; justify-content: space-around;
            &.nav-meau,&.top-one{
                // display: inline-block;white-space:nowrap; 
                li{
                    display: inline-block; vertical-align: top;
                    &.li-one{
                        width: 1.4rem;
                    }
                    .li-content{
                        .num{
                            display: block;
                        }
                        text-align: center;
                    }
                }
                a{
                    font-size: 0; display: flex; flex-direction: column; align-items: center; color: #666666;
                    flex:0 1 auto; width: 1.4rem;flex-shrink:0;
                }
            }
            &.nav-meau{
                flex-wrap: wrap; justify-content: flex-start;
                li{
                    width: 25%; 
                }
            }
            li,a{
                font-size: 0; display: flex; flex-direction: column; align-items: center; color: #666666;
                flex:0 1 auto; width: 1.4rem;
                .num{
                    font-size: .4rem; color: #2f94f1;
                    // &.num0,&.num4{
                    //     color: #499dff;
                    // }
                    // &.num1,&.num3{
                    //     color: #3edbbb;
                    // }
                    // &.num2{
                    //     color: #ff6983;
                    // }
                }
                .p1{
                    font-size: .2rem; line-height: .3rem;text-align: center; white-space:initial;word-wrap: break-word; //word-break:break-all;
                }
                img{
                    width: 60%; margin-bottom: .15rem;
                }
                .p2{
                    font-size: .24rem; line-height: .3rem; text-align: center;
                }
                &.img-pad{
                    box-sizing: border-box; padding: 0 .1rem;
                }
                &.card{
                    box-shadow: 0 0 15px 0px rgba(195, 199, 206, 0.35); width: 31%; margin-bottom: .15rem;
                    box-sizing: border-box; padding: .1rem; border-radius: 3px; position: relative; display: block;
                    .p3{
                        .num{
                            font-size: .32rem;  color: #2f94f1;
                        }
                        // .num0{
                        //     color: #499dff;
                        // }
                        // .num1{
                        //     color: #40dcbc;
                        // }
                        // .num2{
                        //     color: #ffba59;
                        // }
                        // .num3{
                        //     color: #ff6a84;
                        // }
                        // .num4{
                        //     color: #ffc167;
                        // }
                        // .num5{
                        //     color: #46ddbe;
                        // }
                        .zi{
                            font-size: .2rem; margin-left: .1rem;
                        }
                    }
                    .p4{
                        font-size: .2rem;margin-top: .1rem;font-weight: bold;
                    }
                    .tip{
                        box-sizing: border-box; padding: 0 .1rem; border-radius: 3px 0 3px 0;  color: #fff;
                        position: absolute; right: 0; bottom: 0;
                        display: flex; flex-direction: column; align-items: center;
                        span{
                            font-size: .18rem; line-height: .2rem;
                            &:nth-child(2){
                                font-weight: bold;
                            }
                        }
                        // &.tip0{
                        //     background-image: linear-gradient(180deg, #6ab1ff 0%, #5ba7ff 50%, #4c9fff 100%)
                        // }
                        // &.tip1,&.tip5{
                        //     background-image: linear-gradient(180deg, #53dfc3 0%, #44ddbe 50%, #39dbb9 100%)
                        // }
                        // &.tip2,&.tip4{
                        //     background-image: linear-gradient(180deg, #ffc979 0%, #ffc268 50%, #ffbb5a 100%)
                        // }
                        // &.tip3{
                        //     background-image: linear-gradient(180deg, #ff7f94 0%, #ff758c 50%, #ff6780 100%)
                        // }
                        background-image: linear-gradient(180deg, #a7ccf7 0%, #62aafe 100%)
                    }
                }
                
            }
            &.card-box{
                flex-wrap:wrap;
            }
        }
    }
    .bottom-nav{
        position: fixed; width: 100%; bottom: 0; left: 0; background: #fff; height: .85rem;
        box-shadow: 0 -10px 13px -10px rgba(0, 0, 0, 0.35); display: flex; justify-content: space-around; align-items: center;
        font-size: .22rem; color: #666;
        a{
            display: flex; flex-direction: column; align-items: center;color: #666; width: 1.5rem;
            &.act{
                color: #2D8DE2;
                i{
                    color: #2D8DE2;
                }
            }
            i{
                font-size: .40rem; color: #666;
            }
            
        }
    }
    .title-top{
        background-image: linear-gradient(180deg, #1CAEFE 0%, #3482FE 50%, #3F6EFE 100%) ; position: fixed; width: 100%; left: 0;
        height: .85rem; color: #fff;  box-sizing: border-box; padding: 0 .2rem; display: flex; justify-content: space-between; top: 0;
        align-items: center;
        i{
            font-size: 26px; color: #fff;
        }
        a{
            font-size: 22px; color: #fff; display: flex; flex-direction: column; align-items: center;
        }
        span{
            font-size: .24rem;
        }
        img{
            width: 23px; height: 23px; border-radius: 50%;
        }
    }
</style>