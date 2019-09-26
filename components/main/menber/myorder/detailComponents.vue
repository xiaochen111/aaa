<template>
    <div>
        <p class="top-t">
            <span> <img src="../../../../../static/images/icon/dingdan.png" alt="" srcset="">  {{$t('order.orderStatus')}}：{{dataObj['statusName'+language] }}</span>
            <router-link :to="{path:'timenode',query:params}">{{$t('order.gzxq')}}</router-link>
        </p>
        <div class="list-main">
            <p class="tit border-bottom">
                <span class="left">
                    <img :src="dataObj.shippingIdOther" alt="" srcset="">
                    <!-- &nbsp;  {{dataObj.shippingIdCode}} -->
                </span>
                <span class="right">{{$t('order.orderNum')}}：{{dataObj.orderNo}}</span>
            </p>
            <div class="table-li">
                <div class="start">
                    <p class="tit-p">{{dataObj.portStartIdNameEn}}</p>
                    <p>ETD:{{dataObj.estimatedDate | dateInit('MM-DD-YYYY',language)}}</p>
                </div>
                <div class="to">
                    <p>{{$t('order.transit')}}</p>
                    <p>{{dataObj.discPortIdNameEn}}</p>
                </div>
                <div class="end">
                    <p class="tit-p">{{dataObj.portEndIdNameEn}}</p>
                    <p>ETA:{{dataObj.eta | dateInit('MM-DD-YYYY',language)}}</p>
                </div>
            </div>
            <!-- <div class="all-price">
                <span class="orange">1000USD</span>
                <span class="orange">1000CNY</span>
                <span class="blue">费用明细</span>
            </div> -->
        </div>

        <div class="module-two">
            <p class="border-bottom"><span>BKG_NO：</span> {{dataObj.bkgNo}}</p>
            <p class="border-bottom"><span>{{$t('order.quantity')}}： </span>{{dataObj.boxTypeNum}}</p>
            <p class="border-bottom"><span>MBL No.： </span>{{dataObj.blNo}}</p>
            <p class="border-bottom"><span>HBL No.： </span>{{dataObj.hblNo}}</p>
            <p class="border-bottom"><span>{{$t('order.cmhc')}}：</span>{{dataObj.vessel}}/{{dataObj.voyage}}</p>
            <p class="border-bottom"><span>{{$t('order.Transportation')}}：</span>{{dataObj.transportClauseNameCn}}</p>
            <p class="border-bottom"><span>{{$t('order.paymethd')}}：</span>{{dataObj.payMethodIdNameCn}}</p>
            <p class="border-bottom"><span>{{$t('order.Signing')}}：</span>{{dataObj.blMenthodIdNameCn}}</p>
        </div>

        <div class="module-five">
            <p>
                <span class="l-1">{{$t('order.sales')}}:</span>
                <span class="l-2">{{dataObj.xUserName}}</span>
                <span class="l-3">{{$t('order.tel')}}：</span>
                <span class="l-4">{{dataObj.xUserTel}}</span>
            </p>
            <p>
                <span class="l-1">{{$t('order.operation')}}:</span>
                <span class="l-2">{{dataObj.cUserName}}</span>
                <span class="l-3">{{$t('order.tel')}}：</span>
                <span class="l-4">{{dataObj.cUserTel}}</span>
            </p>
            <p>
                <span class="l-1">{{$t('order.Customer')}}:</span>
                <span class="l-2">{{dataObj.kUserName}}</span>
                <span class="l-3">{{$t('order.tel')}}：</span>
                <span class="l-4">{{dataObj.kUserTel}}</span>
            </p>
        </div>

        <div class="module-three">
            <p class="border-bottom tit-pic">
                <img src="../../../../../static/images/icon/shouhuoren.png" alt="" srcset=""> {{$t('order.Consignee')}}
            </p>
            <p class="t-w">
                {{dataObj.consignee}}
            </p>
            <!-- <p class="t-w"><span class="label-w">地址：</span> <span>--</span></p>
            <p class="t-w"><span class="label-w">联系人：</span> <span>--</span></p> -->
        </div>

        <div class="module-three">
            <p class="border-bottom tit-pic">
                <img src="../../../../../static/images/icon/fahuoren.png" alt="" srcset=""> {{$t('order.Consigner')}}
            </p>
            <p class="t-w">
                {{dataObj.shipper}}
            </p>
            <!-- <p class="t-w"><span class="label-w">公司：</span> <span>--</span></p>
            <p class="t-w"><span class="label-w">地址：</span> <span>--</span></p>
            <p class="t-w"><span class="label-w">联系人：</span> <span>--</span></p> -->
        </div>

        <div class="module-three">
            <p class="border-bottom tit-pic">
                <img src="../../../../../static/images/icon/huowuxinxi.png" alt="" srcset=""> {{$t('order.CargoInformation')}}
            </p>
            <p class="t-w"><span>{{$t('order.goodsType')}}：</span> <span>{{dataObj.goodsTypeNameCn}}</span></p>
            <p class="t-w"><span>{{$t('order.Number')}}：</span> <span>{{dataObj.number}}  {{dataObj.packageTypeNameCn}}</span></p>
            <p class="t-w"><span>{{$t('order.weigth')}}：</span> <span>{{dataObj.weight}}KGS</span></p>
            <p class="t-w"><span>{{$t('order.Volume')}}：</span> <span>{{dataObj.volume}}CBM</span></p>
            <p class="t-w"><span>HS CODE：</span> <span>{{dataObj.hsCode}}</span></p>
            <div v-if="dataObj.goodsType==2">
                <p class="t-w"><span>{{$t('order.wxlb')}}：</span> <span>{{dataObj.dgType}}</span></p>
                <p class="t-w"><span>{{$t('order.UNNumber')}}：</span> <span>{{dataObj.unNo}}</span></p>
                <p class="t-w"><span>{{$t('order.wxym')}}：</span> <span>{{dataObj.dgPageNo}}</span></p>
                <p class="t-w"><span>{{$t('order.wpsd')}}：</span> <span>{{dataObj.flashPoint}}</span></p>
                <p class="t-w"><span>PACKING GROUP：</span> <span>{{dataObj.packingGroup}}</span></p>
            </div>
            <div v-if="dataObj.goodsType==3">
                <p class="t-w"><span>{{$t('order.Temperature')}}：</span> <span>{{dataObj.temperature}}</span></p>
                <p class="t-w"><span>{{$t('order.Ventilation')}}：</span> <span>{{dataObj.ventilation}}</span></p>
                <p class="t-w"><span>{{$t('order.Luggage')}}：</span> <span>{{dataObj.containerStuffingDate}}</span></p>
            </div>
            <div v-if="dataObj.goodsType==4">
                <p class="t-w"><span>{{$t('order.Length')}}：</span> <span>{{dataObj.length}}CM</span></p>
                <p class="t-w"><span>{{$t('order.Width')}}：</span> <span>{{dataObj.width}}CM</span></p>
                <p class="t-w"><span>{{$t('order.Height')}}：</span> <span>{{dataObj.height}}CM</span></p>
            </div>
            
        </div>

        <div class="module-three">
            <p class="tit-pic">
                <img src="../../../../../static/images/icon/beizhu.png" alt="" srcset=""> {{$t('order.booknote')}}
            </p>
            <div class="txt-box">
                <textarea name="" id="" cols="30" rows="10"  v-model="dataObj.bookingReq" disabled="disabled"></textarea>
                <!-- <p class="font-s">还能输入<span>{{mark.wordNum}}</span>个字</p> -->
            </div>
        </div>
    </div>
</template>

<script>
    import {mapState} from 'vuex'
    export default {
        props:['dataObj','params'],
        computed:{
            ...mapState(['language']),  //这个是后台中英文翻译的关键
        },
    }
</script>

<style lang="less" scoped>
    .top-t{
        font-size: .24rem;display: flex; justify-content: space-between; align-items: center;margin: 10px; 
        img{
            width: 20px; margin-right: 5px;
        }
        span{
            display: flex; align-items: center; color: #333;
        }
        a{
            color: #3b74fe;
        }
    }
    .list-main{
        height: 2.8rem; box-sizing: border-box; background: #fff; margin: 10px; border-radius: 5px;
        box-shadow: 0 12px 20px -12px rgba(0, 0, 0, 0.35); 
        .tit{
            color:#333;font-size: .3rem;display:flex; justify-content: space-between; align-items: center;box-sizing: border-box; padding:15px 10px;
            .left{
                display:flex;  align-items: center;
                img{
                    width: 50px;
                }
            }
            .right{
                font-size: .24rem;
            }
        }
    }
    .all-price{
        display: flex; justify-content: space-around; align-items: center;
        .orange{
            font-size: .3rem; color: #f59268;
        }
        .blue{
            width: 1.3rem; height: .5rem; color: #fff; background: #368ef0; font-size: .22rem;text-align: center; border-radius: 3px; line-height: .5rem;
        }
    }
    .table-li{
        display: flex; justify-content: space-around; box-sizing: border-box; padding: 10px 0;
        .start{width: 40%;}
        .to{
            width: 20%; background: url('../../../../../static/images/icon/jiantou.png') no-repeat center 45%; background-size: 50%;
        }
        .end{width: 40%;}
        p{
            text-align: center; font-size: .22rem; color:#333; line-height: 25px;overflow: hidden;text-overflow: ellipsis; white-space: nowrap;  
            &.tit-p{
                font-size: .26rem;
            }
        }
    }
    .init-box{
        box-sizing: border-box; padding: .17rem 0 .17rem .17rem; background: #fff; margin-top: .1rem;
    }
    .module-two{
        .init-box;
        p{
            font-size: .24rem; line-height: .7rem; height: .7rem; color: #666;
        }
        .org{
            color: #f6b37f;
        }
    }
    .label-w{
        width: 1.45rem; display: inline-block; font-size: .24rem;
    }
    .tit-pic{
        display: flex; align-items: center;  
        img{
            width: 20px; margin-right: 5px;
        }
    }
    .module-three{
        .init-box;
        p{
             line-height: .65rem; font-size: .24rem;color: #666;
            &.border-bottom{
                margin-bottom: .05rem;
            }
            a{
                color: #f42434; font-size: .2rem;
            }
        }
        .txt-box{
            box-sizing: border-box; padding-right: 10px;
            textarea{
                font-size: .24rem; outline: none; border: 1px solid #cdcccc; width: 100%;border-radius: 4px; box-sizing: border-box; padding: 5px; display: block;
                background: #fff;
            }
            .font-s{
                line-height: 30px; font-size: .22rem; color: #b9b9b9; text-align: right; height: 25px;
            }
            span{
                color: #f42434;
            }
        }
        input{
            width: 4.4rem; height: .55rem; border:1px solid #cdcccc; outline: none; border-radius: 3px; text-indent: 5px;
        }
    }
    .module-five{
        .init-box;
        p{
            display: flex;  height: .55rem; flex-direction: row ; align-items: center;
            span{
                display: inline-block; font-size: .24rem;  color: #666;
            }
            .l-1{width: 1.4rem;}
            .l-2{width: 1.3rem;}
            .l-3{width: 1.2rem;box-sizing: border-box; text-align: right; padding-right: .3rem;}
        }
    }
</style>