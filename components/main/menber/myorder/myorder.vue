<template>
    <div id="myorder">
        <nav-bar :title="$t('order.myorder')"></nav-bar>
        <div class="fix-search">
            <form action="/" class="ser-top">
                <van-search
                    v-model="value"
                    :placeholder="$t('order.orderNum')"
                    background="#fff"
                    @search="[likeSearch('likeNo',value)]"
                    @clear-callback="clearfn"
                >
                </van-search>
            </form>
            <div class="tab-filter">
                <span v-for="(item,index) in statusArr['statusStr']" :key="index" :class="tabfilter==(item.parmes)?'act':''" @click="choiseFilter(item.parmes)">{{item['name'+language]}}</span>
            </div>
        </div>


        <scroll ref="scroll" class="recommend-content" :data="list" :pullup=true @scrollToEnd="onLoad">
            <div>

                <div class="list-main" v-for="(item,index) in list" :key="index" @click="toDetail(item.id)">
                    <p class="border-bottom p-tit">
                        <span class="name-b"><img :src="item.shippingIdOther" :alt="item.shippingIdCode" srcset="">&nbsp; {{item.orderNo}}</span>
                        <span class="f-r">{{item['statusName'+language]}}</span>
                    </p>

                    <div class="main-li">
                        <div class="qzm">
                            <span class="qi">{{item['portStartIdNameEn']}}</span>
                            <!-- <span class="zhong">{{$t('order.transit')}}</span> -->
                            <span class="zhong"></span>
                            <span class="mu">{{item['portEndIdNameEn']}}</span>
                        </div>
                        <!-- <p class="zhong-bottom">{{item.discPortIdNameEn}}</p> -->
                        <p class="data-str border-bottom">
                            <span class="data-item">ETD:{{item.estimatedDate|dateInit('MM-DD-YYYY',language)}}</span>
                            <span class="data-middle"></span>
                            <span class="data-item">ETA:{{item.eta|dateInit('MM-DD-YYYY',language)}}</span>
                        </p>

                        <p class="four-box">
                            <span>{{$t('order.cmhc')}}：{{item.vessel}}/{{item.voyage}}</span>
                            <span>{{item['goodsTypeName'+language]}}</span>
                            <span>{{item.boxTypes}}</span>
                            <span>
                                <i class="iconfont icon-haiyun" @click.stop="linkto(item.id)"></i>
                            </span>
                        </p>
                    </div>

                </div>

                <p class="loading" v-show="loading">{{loadTxt||$t('common.loading')}}</p>
                <p class="loading" v-show="!loading">{{loadTxt||$t('common.loading')}}</p>

            </div>
        </scroll>

        <ul class="bot-filter">
            <li @click="show=true">
                <i class="iconfont icon-dingdanzhuangtai" style="font-size:24px; margin-bottom:4px; margin-top:3px;"></i>
                <span>{{$t('order.Status')}}</span>
            </li>
            <li @click="rankBt('kh')">
                <i :class="botfilter.kh.rank==0?'iconfont icon-kaihangri-jiang1':'iconfont icon-kaihangri-sheng'"></i>
                <span>{{$t('order.sailing')}}</span>
            </li>
            <li @click="rankBt('cj')">
                <i :class="botfilter.cj.rank==0?'iconfont icon-chuangjianriqi-jiang':'iconfont icon-chuangjianriqi'"></i>
                <span>{{$t('order.createDate')}}</span>
            </li>
        </ul>
        <van-actionsheet v-model="show"  :actions="statusArr.status"  />
    </div>
</template>

<script>
    import { Search,Actionsheet} from 'vant';
    import navBar from '@/components/common/navBar'
    import Scroll from '@/components/common/scroll'
    import {mapState,mapMutations} from 'vuex'
    import { orderList } from '@/api/sysapi';
    export default {
        name:'myorder',
        components:{
            [Search.name]:Search,
            [Actionsheet.name]:Actionsheet,
            Scroll,
            navBar,
        },
        data(){
            return {
                show:false,
                statusArr:{
                    status:[{name:'All',parmes:'',callback:this.select},{name:'RECEIVED',parmes:'7',callback:this.select},
                    {name:'BKG PENDING',parmes:'1',callback:this.select},{name:'BKG TO CARRIER',parmes:'2',callback:this.select}
                    ,{name:'BKG CONFIRMED',parmes:'3',callback:this.select},{name:'ONBOARD',parmes:'4',callback:this.select}
                    ,{name:'BKG CANCELLED',parmes:'5',callback:this.select},{name:'ARRIVAL',parmes:'6',callback:this.select}],
                    statusStr:[{nameCn:'未开航',nameEn:'PROCESSING',parmes:'1,2,3'},{nameCn:'已开航',nameEn:'ON BOARD',parmes:'4'},{nameCn:'已抵达',nameEn:'ARRIVAL',parmes:'6'}],
                },
                value:'',
                list:[],
                filterParams:{
                    pageNo:1
                },
                finished:false,
                loadTxt:'',
                loading: false,
                tabfilter:'',
                botfilter:{
                    kh:{
                        rank:0,
                        ranksql:['o.ESTIMATED_DATE ASC','o.ESTIMATED_DATE DESC']
                    },
                    cj:{
                        rank:0,
                        ranksql:['b.CREATE_TIME ASC','b.CREATE_TIME DESC']
                    }
                },
                begFlag:false
            }
        },
        computed:{
            ...mapState(['language']),  //这个是后台中英文翻译的关键
        },
        methods:{
            ...mapMutations(['keepAlivedef']),
            select(item){
                this.show = false;
                this.choiseFilter(item.parmes);
            },
            choiseFilter(str){
                this.tabfilter = str;
                this.filterParams['statusStr'] = str;
                this.setSearch()
            },
            likeSearch(key,value){
                this.filterParams[key] = value;
                this.setSearch()
            },
            rankBt(key){
                let obj = this.botfilter[key];
                this.filterParams.orderByClause = obj.ranksql[obj.rank]
                this.setSearch();
                obj.rank = !obj.rank?1:0;
            },
            toDetail(id){
                this.$router.push({
                    path:'/myorderDetail',
                    query:{id:id},//传参数
                })
            },
            getbookingList(parmes={}){
                this.begFlag = false
                orderList(parmes).then(res=>{
                    this.filterParams.pageNo++
                    this.list = this.list.concat(res.data); 
                    if(res.data.length<10){
                        this.loadTxt = this.language=="Cn"? '数据全部加载完成':'The data is fully loaded';
                        this.finished = true;
                    }
                    if(!res.data.length){
                        this.loadTxt = this.language=="Cn"?'暂无数据。。。':'Temporarily no data';
                    }
                    this.loading = false;
                     this.begFlag = true
                })
            },
            onLoad(){
                this.loading = true;
                this.loadTxt = this.language=="Cn"?'加载中。。':'loading..';
                if(this.finished){
                    this.loading = false;
                    this.loadTxt = this.language=="Cn"? '数据全部加载完成':'The data is fully loaded';
                    return
                }
                if(this.begFlag){
                    this.getbookingList(this.filterParams);
                }
            },
            setSearch(){
                this.list = [];
                this.filterParams.pageNo = 1;
                this.finished = false;
                this.loadTxt = this.language=="Cn"?'加载中。。':'loading..';
                clearTimeout(this.searchTimeout)
                this.searchTimeout = setTimeout(()=>{
                    this.getbookingList(this.filterParams);
                },200)
            },
            linkto(id){
                // this.$router.push({
                //     path:'/timenode',
                //     query:{id:id},//传参数
                // })
                this.$router.push({
                    path:'/myorderDetail',
                    query:{id,act:4},//传参数
                })
            },
            clearfn(){
                this.filterParams['likeNo'] = '';
                this.setSearch()
            }
        },
        created() {
            let params = this.$route.query;
            if(Object.keys(params).length){
                if(params.statusStr>=0){
                    let statusStr = this.statusArr.statusStr[params.statusStr].parmes;
                    this.choiseFilter(statusStr)
                }else{
                    this.value = params.subBookingNo
                    this.getbookingList(params);
                }
            }else{
                this.getbookingList()
            }
        },
        mounted() {
           this.keepAlivedef('myorder') 
        },
        
    }
</script>

<style lang="less" scoped>
    #myorder{
        padding-top: 46px; padding-bottom: 50px;  overflow: hidden;
        height: calc(~'100% - 88px - 46px - .2rem');
        // height: calc(~'100% - 88px - 46px');
    }
    .tab-filter-box{
        background: #fff; position: relative; width: 100%; overflow: hidden; height: .68rem;margin-bottom: .1rem;
        .nav{
            height: 100%; width: .45rem; background: rgba(255, 255, 255, .9); position: absolute; right: 0; top: 0; text-align: center;
            box-shadow: -5px 0 5px -5px #ccc;
            i{
                color: #666; font-size: .4rem; vertical-align: top;
            }
            .icon-gengduo:before{
                position: absolute; top: 5%;
            }
        }
    }
    .recommend-content{
        height: calc(~'100% - 32px');
    }

    .tab-filter{
        background: #fff;  width: 100%; display: flex; justify-content: space-around; margin-bottom: .1rem;
        span{
            font-size: .24rem; line-height: .55rem; 
            &.act{
                border-bottom: 2px solid #368ef0; color: #368ef0;
            }
        }
    }
    .bot-filter{
        height: 50px; position: fixed;left: 0; bottom: 0; width: 100%; display: flex;justify-content: space-around; 
        background: #fff; font-size: .22rem; align-items: center;box-shadow: 0 -10px 13px -10px rgba(0,0,0,0.35);
        li{
            display: flex; align-items: center; flex-direction: column; width: 50%; color: #666;
            img{
                height: .45rem;
            }
            i{
                font-size: .55rem;
            }
            span{
                margin-top: -.1rem;
            }
        }
    }
    .loading{
        line-height: .44rem; font-size: .18rem; text-align: center;color: #666;
    }
    .list-main{
        background: #fff; box-sizing: border-box; padding-left: .2rem; margin-bottom: .1rem;
        .p-tit{
            display: flex; justify-content: space-between; align-items: center;
            box-sizing: border-box;  padding:.1rem .2rem; padding-left: 0;
            .name-b{
                font-size: .24rem; color: #333; display: flex; align-items: center;
                img{
                    width: .5rem; height: .5rem;
                }
            }
            .f-r{
                font-size: .2rem; color: #f36363;
            }
        }
        .main-li{
            box-sizing: border-box; padding-right: .2rem;
        }

        .qzm{
            display: flex;  padding: .15rem 0 0; align-items: center;
            span{text-align: center;}
            .qi,.mu{
                // display: inline-block; width: 2.5rem; 
                font-size: .26rem; color: #333;flex-grow:3;width: 0;
            }
            .zhong{
                color: #666;  font-size: .18rem; background: url('../../../../../static/images/icon/jiantou.png') no-repeat center; background-size: 80%;
                height: .45rem;flex-grow:1;width: 0;
            }
        }
        .zhong-bottom{
            text-align: center;color: #666; font-size: .18rem; 
        }

        .data-str{
            display: flex; justify-content: space-around; align-items: flex-end; font-size: .22rem; color: #666;padding: 0 0 .15rem;
            .data-item{
                flex-grow:2; text-align: center; overflow: hidden; width: 0;
            }
            .data-middle{
                flex-grow:1;width: 0;
            }
        }

        .four-box{
            display: flex; justify-content: space-between; align-items: center; font-size: .2rem; color: #666; padding: .05rem 0;
            span{
                width: 0;  text-align: center; overflow: hidden;text-overflow: ellipsis; white-space: nowrap;   
            }
            span:first-child{
                flex-grow: 4;text-align: left; 
            }
            span:nth-child(2){
                text-align: left;flex-grow:1;
            }
            span:nth-child(3){
                text-align: left;flex-grow:2;
            }
            span:last-child{
                flex-grow: 1;
            }
            img{
                width: .4rem; height: .4rem; 
            }
            i{
                font-size: .35rem; color: #368ef0;
            }
        }
    }
</style>