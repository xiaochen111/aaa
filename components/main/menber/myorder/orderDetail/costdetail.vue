<template>

    <div class="contdetail">
        <nav-bar title="费用详情"></nav-bar>
        <scroll  ref="scroll" class="recommend-content" :data="list">
            <div>
                <div class="all-price">
                    <span class="tit">{{$t('common.Oceanfreight')}}</span>
                    <span class="unit">USD</span>
                    <span class="price">{{allInfo.totalUsd}}</span>
                </div>
                <ul>
                    <li v-for="(item,index) in list" :key="index">
                        <p class="t">
                            <span>{{item['chargeId'+language]}}</span>
                            <span>{{item.totalPrice}} </span>
                            <span>{{item['currency'+language]}}</span>
                        </p>
                        <p class="price" v-if="item.unit==2">{{item.price20}}/{{item.price40}}/{{item.price40hq}}</p>
                        <p class="price" v-if="item.unit==1">{{item.billPrice}}</p>
                    </li>
                </ul>
            </div>
        </scroll>
    </div>
</template>

<script>
    import navBar from '@/components/common/navBar'
    import Scroll from '@/components/common/scroll'
    import {orderToBookingDetail} from '@/api/getData'
    import {mapState} from 'vuex'    
    export default {
        components:{
            navBar,
            Scroll
        },
        data(){
            return {
                list:[],
                allInfo:{},
            }
        },
        computed:{
            ...mapState(['language']),  //这个是后台中英文翻译的关键
        },
        created() {
            let params = this.$route.query;
            orderToBookingDetail(params).then(res=>{
                this.list = res.data.charList;
                this.allInfo = res.data;
            })
        },
    }
</script>

<style lang="less" scoped>
    .contdetail{
        box-sizing: border-box; padding-top: 46px; height: 100%; overflow: hidden;  
    }
    .all-price{
        box-sizing: border-box; padding: .3rem .2rem; background: #fff; font-size: .26rem; display: flex; align-items: center;margin-bottom: .2rem;
        .tit{
            color: #333; margin-right: .2rem;
        }
        .unit,.price{
            font-size: .28rem; font-weight: bold; margin-right: .15rem; color: #fe624d;
        }
    }

    ul{
        li{
          box-sizing: border-box; padding: .2rem .2rem;   background: #fff; font-size: .26rem;  color: #666; margin-bottom: .2rem;
          p{
              padding: .05rem 0;
          }
          .t{
              span{
                  margin-right: 10px;
              }
          }
          .price{
              font-size: .24rem;
          }
        }
    }
</style>