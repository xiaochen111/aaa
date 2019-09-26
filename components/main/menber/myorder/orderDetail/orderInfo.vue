<template>
    <div v-if="loadbox">
        <div v-if="nodate" class="recommend-content">
            <div>
                <!-- <div class="list-main">
                    <div class="all-detail">
                        <div class="all-box">
                            <span>{{$t('common.TotalAssets')}}</span>
                            <span>USD {{allinfo.totalUsd}}</span>
                            <span>CNY {{allinfo.totalCny}}</span>
                        </div>
                        <router-link class="detail" :to="{path:'/costdetail',query:params}">
                            <i class="iconfont icon-feiyongxiangqing"></i>
                        </router-link>
                    </div>
                </div> -->

                <div class="module">
                    <ul class="clearfix">
                        <li><span class="fw">{{$t('common.WorkNo')}}：</span> {{booking.subBookingNo}}</li>
                        <li><span class="fw">{{$t('common.POL')}}：</span> {{order['portStartIdEn']}}</li>
                        <li><span class="fw">{{$t('common.POD')}}：</span> {{order['portEndIdEn']}}</li>
                        <li><span class="fw">{{$t('common.PlaceOfReceipt')}}：</span> {{order['discPortIdEn']}}</li>
                        <li><span class="fw">ETD：</span> {{order.estimatedDate | dateInit('MM-DD-YYYY',language)}}</li>
                        <li><span class="fw">ETA：</span> {{order.eta | dateInit('MM-DD-YYYY',language)}}</li>
                        <li><span class="fw">{{$t('common.shipCom')}}：</span> {{order.shippingIdCode}}</li>
                        <li><span class="fw">{{$t('common.Cutofftime')}}：</span> {{booking.cutoffTime | dateInit('MM-DD-YYYY',language)}}</li>
                        <li><span class="fw">{{$t('common.Cutordertime')}}：</span> {{booking.cutOrderTime | dateInit('MM-DD-YYYY',language)}}</li>
                        <li><span class="fw">{{$t('common.Portcutofftime')}}：</span> {{booking.cutPortTime | dateInit('MM-DD-YYYY',language)}}</li>
                        <li><span class="fw">{{$t('common.typevolume')}}：</span> {{booking.boxTypes}}</li>
                        <li><span class="fw">{{$t('order.cmhc')}}：</span> {{order.vessel}}/{{order.voyage}}</li>
                        <!-- <li>{{$t('common.Portcode')}}： {{booking.portCode}}</li> -->
                        <!-- <li>{{$t('common.Shipagency')}}： {{booking.shipAgency}}</li> -->
                    </ul>
                </div>


                <!-- <div class="module">
                    <ul class="clearfix">
                        <li>客服：季艺华* 2319  61322319</li>
                        <li>订单状态：<span class="font-red">处理中</span>  </li>
                        <li>单证进度：M单-草稿     H单-草稿      预配-已提交</li>
                    </ul>
                </div> -->

                

                <div class="module" >
                    <p class="border-bottom tit-pic">
                        <i class="iconfont icon-dingdan"></i>
                        DOC
                    </p>
                    <ul>
                        <!-- 如果是直客 m单附件不显示 m单：fileType=2 -->
                        <li v-for="(item,index) in fileList" :key="index" v-show="!((item.fileType==2)&&(loginflag.infor.bussiType == 2))">
                            <span class="fw nowrap">{{item['fileType'+language]}}：</span>  
                            <a class="text-underline" v-if="isiOS" @click="viewfile(item)">{{item.fileName}}</a>
                            <a v-else :href="`${baseurl}${item.rootPathCn}/${item.filePath}`" download="download" class="download text-underline">{{item.fileName}}</a>
                            <i class="iconfont icon-xiazai" v-if="isAndroid"></i>
                        </li>
                    </ul>
                </div>

                <div class="module" >
                    <p class="border-bottom tit-pic">
                        <i class="iconfont icon-dingdan"></i>
                        D/N INFO
                    </p>
                    <ul>
                        <li v-for="(item,index) in html5List" :key="index">
                            <span class="fw nowrap">{{item['fileType'+language]}}：</span>  
                            <a class="text-underline" v-if="isiOS" @click="viewfile(item)">{{item.fileName}}</a>
                            <a v-else :href="`${baseurl}${item.rootPathCn}/${item.filePath}`" download="download" class="download text-underline">{{item.fileName}}</a>
                            <i class="iconfont icon-xiazai" v-if="isAndroid"></i>
                        </li>
                    </ul>
                </div>

                <div class="module" >
                    <p class="border-bottom tit-pic">
                        <i class="iconfont icon-dingdan"></i>
                        P.LIST&INVOICE:
                    </p>
                    <ul>
                        <li v-for="(item,index) in appointFileList" :key="index">
                            <a class="text-underline" v-if="isiOS" @click="viewfile(item)">{{item.fileName}}</a>
                            <a v-else :href="`${baseurl}${item.rootPathCn}/${item.filePath}`" download="download" class="download text-underline">{{item.fileName}}</a>
                            <i class="iconfont icon-xiazai" v-if="isAndroid"></i>
                        </li>
                    </ul>
                </div>

                <div class="module">
                    <p class="border-bottom tit-pic">
                        <i class="iconfont icon-dingdan"></i>
                        {{$t('common.Trandpaymentinfo')}}
                    </p>
                    <ul class="clearfix">
                        <li><span class="fw">{{$t('common.Paymentmethod')}}：</span> {{booking['payMethodId'+language]}}</li>
                        <li><span class="fw">{{$t('common.Signingmethod')}}：</span> {{booking['blMenthodId'+language]}}</li>
                        <li><span class="fw">{{$t('common.Transportation')}}：</span> {{booking['transportClause'+language]}}</li>
                    </ul>
                </div>

                <div class="module">
                    <p class="border-bottom tit-pic">
                        <!-- <img src="../../../../../../static/images/icon/order/kefu.png" alt="" srcset="">  -->
                        <i class="iconfont icon-zhiwei"></i>
                        {{$t('common.BookingRemark')}}
                    </p>
                    <ul class="clearfix">
                        <!-- <li>{{$t('common.Contactscompanyname')}}： {{booking['customerId'+language]}}</li> -->
                        <!-- <li>{{$t('common.Contactsname')}}： {{booking.contract}}</li>
                        <li>{{$t('common.Contactsphone')}}： {{booking.mobile}}</li>
                        <li>{{$t('common.Contactsemail')}}： {{booking.email}}</li> -->
                        <li><span class="fw">{{$t('common.salesman')}}：</span> {{allinfo.xUserName}}</li>
                        <li><span class="fw">{{$t('common.salestel')}}：</span> {{allinfo.xUserTel}}</li>
                        <li><span class="fw">{{$t('common.salesemail')}}：</span> {{allinfo.xUserEmail}}</li>
                        <li><span class="fw">{{$t('common.Customerservice')}}：</span> {{allinfo.kUserName}}</li>
                        <li><span class="fw">{{$t('common.Customerservicetel')}}：</span> {{allinfo.kUserTel}}</li>
                        <li><span class="fw">{{$t('common.Customerserviceemail')}}：</span> {{allinfo.kUserEmail}}</li>
                        <li><span class="fw">{{$t('common.operator')}}：</span> {{allinfo.kUserName}}</li>
                        <li><span class="fw">{{$t('common.operatortel')}}：</span> {{allinfo.kUserTel}}</li>
                        <li><span class="fw">{{$t('common.operatoremail')}}：</span> {{allinfo.kUserEmail}}</li>
                    
                    </ul>
                    <!-- <div class="text-area bor-b">
                        <div class="textarea">
                            {{booking.bookingReq}}
                        </div>
                    </div> -->
                </div>

                <div class="module">
                    <p class="border-bottom tit-pic">
                        <i class="iconfont icon-hezuo"></i>
                        {{$t('common.Parties')}}
                    </p>
                    <ul class="clearfix">
                        <li class="tit">{{$t('order.Consigner')}}： </li>
                        <!-- <li>{{$t('common.Name')}}： {{booking.fName}}</li> -->
                    </ul>
                    <div class="text-area bor-b">
                        <div class="textarea">
                            {{booking.fAddress}}
                        </div>
                    </div>
                    <ul class="clearfix">
                        <li class="tit">{{$t('order.Consignee')}}： </li>
                        <!-- <li>{{$t('common.Name')}}：{{booking.sName}}</li> -->
                    </ul>
                    <div class="text-area bor-b">
                        <div class="textarea">
                            {{booking.sAddress}}
                        </div>
                    </div>
                    <ul class="clearfix">
                        <li class="tit">{{$t('common.Notify')}}： </li>
                        <!-- <li>{{$t('common.Name')}}：{{booking.tName}}</li> -->
                    </ul>
                    <div class="text-area bor-b">
                        <div class="textarea">
                            {{booking.tAddress}}
                        </div>
                    </div>
                </div>

                <div class="module">
                    <div class="tit-table">{{$t('common.typevolume')}}</div>
                    <div class="content" ref="content1">
                        <div class="long">
                            <table border="1" >
                                <thead>
                                    <tr>
                                        <th width="100">{{$t('common.Boxtype')}}</th>
                                        <th width="100">{{$t('common.Quantity')}}</th>
                                        <th width="100">{{$t('common.Description')}}</th>
                                        <th width="100">{{$t('common.SOC')}}</th>
                                        <th width="100">{{$t('common.BoxNo')}}</th>
                                    </tr>
                                </thead>
                                <tbody>
                                    <tr v-for="(item,index) in containerNumlist" :key="index">
                                        <td>{{item['ctnType'+language]}}</td>
                                        <td>{{item.number}}</td>
                                        <td>{{item.remark}}</td>
                                        <td>{{item['socFlag'+language]}}</td>
                                        <td>{{item.ctnNo}}</td>
                                    </tr>
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>

                <div class="module">
                    <div class="tit-table">{{$t('common.Agentcontacts')}}</div>
                    <div class="content" ref="content2">
                        <div class="long">
                            <table border="1" >
                                <thead>
                                    <tr>
                                        <th width="100">{{$t('common.role')}}</th>
                                        <th width="100">{{$t('common.xmname')}}</th>
                                        <th width="140">{{$t('common.tel')}}</th>
                                        <th width="180">{{$t('common.phone')}}</th>
                                        <th >{{$t('common.email')}}</th>
                                    </tr>
                                </thead>
                                <tbody>
                                    <tr v-for="(item,index) in agentContactsList" :key="index">
                                        <td>{{item.role}}</td>
                                        <td>{{item.name}}</td>
                                        <td>{{item.mobile}}</td>
                                        <td>{{item.tel}}</td>
                                        <td>{{item.email}}</td>
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
                            {{bookingGoods.marks}}
                        </div>
                    </div>
                    <ul class="clearfix">
                        <li class="tit">{{$t('common.Tradename')}}： </li>
                    </ul>
                    <div class="text-area bor-b">
                        <div class="textarea">
                            {{bookingGoods.goods}}
                        </div>
                    </div>
                    <ul class="clearfix">
                        <li><span class="fw">{{$t('order.goodsType')}}：</span> {{bookingGoods['goodsType'+language]}}</li>
                        <li><span class="fw">{{$t('order.Number')}}：</span> {{bookingGoods.number}} </li>
                        <li><span class="fw">{{$t('order.packtype')}}：</span> {{bookingGoods['packageType'+language]}}</li>
                        <li><span class="fw">{{$t('order.weigth')}}：</span> {{bookingGoods.weight}}KGS</li>
                        <li><span class="fw">{{$t('order.Volume')}}：</span> {{bookingGoods.volume}}CBM</li>
                        <!-- <li><span class="fw">HS CODE：</span> {{bookingGoods.hsCode}}</li> -->
                        <div v-if="bookingGoods.goodsType==2">
                            <li><span class="fw">{{$t('order.wxlb')}}：</span> {{bookingGoods.dgType}}</li>
                            <li><span class="fw">{{$t('order.UNNumber')}}：</span> {{bookingGoods.unNo}}</li>
                            <li><span class="fw">{{$t('order.wxym')}}：</span> {{bookingGoods.dgPageNo}}</li>
                            <li><span class="fw">{{$t('order.wpsd')}}：</span> {{bookingGoods.flashPoint}}</li>
                            <li><span class="fw">PACKING GROUP：</span> {{bookingGoods.packingGroup}}</li>
                        </div>
                        <div v-if="bookingGoods.goodsType==3">
                            <li><span class="fw">{{$t('order.Temperature')}}：</span> {{bookingGoods.temperature}}</li>
                            <li><span class="fw">{{$t('order.Ventilation')}}：</span> {{bookingGoods.ventilation}}</li>
                            <li><span class="fw">{{$t('order.Luggage')}}：</span> {{bookingGoods.containerStuffingDate}}</li>
                        </div>
                        <div v-if="bookingGoods.goodsType==4">
                            <li><span class="fw">{{$t('order.Length')}}：</span> {{bookingGoods.length}}CM</li>
                            <li><span class="fw">{{$t('order.Width')}}：</span> {{bookingGoods.width}}CM</li>
                            <li><span class="fw">{{$t('order.Height')}}：</span> {{bookingGoods.height}}CM</li>
                        </div>
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
                allinfo:{},
                booking:{},
                order:{},
                baseurl:'',
                containerNumlist:[],
                bookingGoods:{},
                appointFileList:[],
                agentContactsList:[],
                fileList:[],
                html5List:[],
                nodate:true,
                loadbox:false,
                isiOS:false
            }
        },
        created() {
            this.getorderToBookingDetail(this.params)
            let u = navigator.userAgent;
            this.isiOS = !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/);
        },
        methods:{
            viewfile(item){
                item.baseurl = this.baseurl;
                this.$router.push({
                    name:'fileview',
                    params:item,
                })
            },
            getorderToBookingDetail(params){
                toDetailBooking('toDetailBooking',params).then(res=>{
                    this.allinfo = res.data||{};
                    this.booking = res.data.booking||{};
                    this.baseurl = res.data.aliPath;
                    this.order = res.data.order||{};
                    this.containerNumlist = res.data.containerNumlist||[];
                    this.appointFileList = res.data.appointFileList || [];
                    this.fileList = res.data.fileList||[];
                    this.bookingGoods = res.data.bookingGoods||{};
                    this.agentContactsList = res.data.agentContactsList||[];
                    this.html5List = res.data.html5List || [];
                    this.loadbox = true;
                    if(!Object.keys(this.allinfo).length&&!Object.keys(this.booking).length&&!Object.keys(this.order).length&&!Object.keys(this.bookingGoods).length&&!this.containerNumlist.length){
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
            initScrollX(){ //better-scroll 横线滚动条
                ['content1','content2'].forEach(item=>{
                    new BScroll(this.$refs[item], { startX:0,lick:true,scrollX:true,scrollY:false,})
                })
            },
            scrolldef(){
                this.initScrollX();
                let wrapper = document.querySelector('.recommend-content');
                this.wrapper = new BScroll(wrapper,{
                    click:true,
                })
            },
            
        },
        computed:{
            ...mapState(['language','loginflag']),  //这个是后台中英文翻译的关键
        },
    }
</script>

<style lang="less" scoped>
    .recommend-content{
        position: absolute; left: 0; right: 0;  height: calc(~'100% - 96px');
    }
    .module ul li a.text-underline{
        text-decoration: underline; color: #1d89e4;
    }
    .icon-xiazai{
         color: #1d89e4; margin-left: 10px;
    }
    .nowrap{
        white-space:nowrap;
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

    .list-main{
        box-sizing: border-box; padding:.1rem .2rem; background: #fff;
        .table-li{
             display: flex;
            .start,.end{
                width: 40%; display: flex; align-items: center; justify-content: space-around;
                p{
                    overflow: hidden;text-overflow: ellipsis; white-space: nowrap; color: #333;
                }
            }
            .to{
                width: 20%; background: url('../../../../../../static/images/icon/jiantou.png') no-repeat center 45%; background-size: 50%;
                p{
                    white-space: nowrap; color:#666; text-align: center;
                }
            }
            p{
                text-align: center; font-size: .22rem;  line-height: 35px;
                &.tit-p{
                    font-size: .26rem;
                }
            }
        }
        .date-three{
            color: #666; font-size: .22rem; display: flex; justify-content: space-between;  align-items: center; height:25px; margin-top: -5px;
        }
        .all-detail{
            display: flex; justify-content: space-between; align-items: center; font-size: .24rem; height: 35px;
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
            .detail{
                i{
                    font-size: .38rem; color: #1d89e4; 
                }
            }
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
                a{
                   font-size: .24rem; color: #666; 
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
        border-collapse:collapse; font-size: .24rem; color: #666; border:1px solid #cdcccc; table-layout: fixed; width: 700px;
        th,td{
            box-sizing: border-box; padding: .15rem 0; text-align: center;
        }
    }
    .content{
        overflow: hidden;width: 100%;
        .long{
            width: 700px;
        }
    }
</style>