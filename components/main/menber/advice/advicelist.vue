<template>
    <div id="advicelist">
        <nav-bar :title="$t('common.advice')"></nav-bar>
        <form action="/" class="ser-top">
            <van-search
                v-model="value"
                :placeholder="$t('common.billNo')"
                background="#fff"
                @search="onKeyup"
            >
            </van-search>
        </form>
        <!-- 条件筛选 -->
        <div ref="filter" class="tab-filter-box">
            <div class="tab-filter">
                <span v-for="(item,index) in statusArr['status'+language]" :key="index" :class="tabfilter==(index+1)?'act':''" @click="choiseFilter(index+1)">{{item}}</span>
            </div>
            <span class="nav">
                <i class="iconfont icon-gengduo"></i>
            </span>
        </div>

        <div class="box-list">
            <scroll ref="scroll" class="recommend-content" :data="list" :pullup=true @scrollToEnd="onLoad">
                <div>
                    <div class="list-main" v-for="(item,index) in list" :key="index" @click="toDetail(item.id)">
                        <h4 class="border-bottom">
                            <p>
                            <!-- <img src="../../../../../static/images/menber/advice/advice-num.png" alt=""> -->
                            <i class="iconfont icon-dingdan"></i>
                            <span>{{$t('common.accountNo')}}：{{item.accountNo}}</span> 
                            </p>
                            <span class="red">{{statusArr['status'+language][item.status-1]}}</span>
                        </h4>
                        <div class="content">
                            <ul class="clearfix">
                                <li>{{$t('common.billType')}}:{{item['shouzhi'+language]}}</li>
                                <li>{{$t('common.JobNo')}}：{{item.workNo}}</li>
                                <li>{{$t('common.Currency')}}：{{item['currency'+language]}}</li>
                                <li>{{$t('common.InvoiceTime')}}：{{item.kpTime | dateInit('MM-DD-YYYY',language)}}</li>
                            </ul>
                            <p>
                                <span>{{$t('common.Amount')}}</span>
                                <span>{{item.amount}}</span>
                            </p>
                        </div>
                    </div>
                    <p class="loading" v-show="loading">{{loadTxt||$t('common.loading')}}</p>
                    <p class="loading" v-show="!loading">{{loadTxt||$t('common.loading')}}</p>
                </div>
            </scroll>
        </div>

    </div>
</template>

<script>
    import { NavBar,Icon,Search,} from 'vant';
    import Scroll from '@/components/common/scroll'
    import navBar from '@/components/common/navBar'
    import {queryAccountPage} from '@/api/getData';
    import BScroll from 'better-scroll'
    import {mapState} from 'vuex'
    export default {
        components:{
            [NavBar.name]:NavBar,
            [Search.name]:Search,
            Scroll,
            navBar
        },
        data(){
            return {
                value:'',
                tabfilter:'',
                statusArr:{
                    statusCn:['草稿','待审核','已审核','作废','红冲','锁定'],
                    statusEn:['DRAFT','TO AUDIT','AUDITED','CANCELLATION','RED DASHED','LOCK'],
                },
                list:[],
                finished:false,
                loadTxt:'',
                loading: false,
                filterParams:{
                    pageNo:1
                },
            }
        },
        created() {
            this.getQueryAccountPage();
        },
        computed:{
            ...mapState(['language']),  //这个是后台中英文翻译的关键
        },
        methods:{
            initScrollX(){ //better-scroll 横线滚动条
                if (!this.scroll){
                    this.scroll=new BScroll(this.$refs.filter, {
                        startX:0,
                        click:true,
                        scrollX:true,
                        scrollY:false,
                        // eventPassthrough:'vertical'
                    })
                }else{
                    this.scroll.refresh()
                }
            },
            toDetail(id){
                this.$router.push({
                    path:'/adviceDetail',
                    query:{id:id},//传参数
                })
            },
            onKeyup(){
                this.filterParams.likeNo = this.value;
                this.setSearch();
            },
            onCancel(){
                this.value = '';
                this.filterParams.likeNo = this.value;
                this.setSearch();
            },
            
            onLoad(){
                this.loading = true;
                this.loadTxt = this.language=="Cn"?'加载中。。':'loading..';
                if(this.finished){
                    this.loading = false;
                    this.loadTxt = this.language=="Cn"? '数据全部加载完成':'The data is fully loaded';
                    return
                }
                this.getQueryAccountPage(this.filterParams);
            },
            getQueryAccountPage(parmes={}){
                queryAccountPage(parmes).then(res=>{
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

                })
            },
            setSearch(){
                this.list = [];
                this.filterParams.pageNo = 1;
                this.finished = false;
                this.getQueryAccountPage(this.filterParams);
            },
            choiseFilter(str){
                this.tabfilter = str
                this.filterParams.status = str;
                this.setSearch();
            },
        },
        mounted(){
            this.initScrollX();
        },
    }
</script>

<style lang="less" scoped>
    #advicelist{
        padding-top: 46px; height: calc(~'100% - 46px'); overflow: hidden;
    }
    .box-list{
        height: calc(~'100%  - 1.4rem'); overflow: hidden;
    }
    .tab-filter-box{
        background: #fff; position: relative;
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
    .tab-filter{
        display: flex; justify-content: space-around; background: #fff; margin-bottom: .1rem;width: 9rem; box-sizing: border-box; padding-right:.4rem;
        span{
            display: inline-block; font-size: .24rem; line-height: .55rem; //padding: 0 .13rem;
            &.act{
                border-bottom: 2px solid #368ef0; color: #368ef0;
            }
        }
    }
    .loading{
        line-height: .44rem; font-size: .18rem; text-align: center;color: #666;
    }
    .list-main{
        background: #fff; box-sizing: border-box; padding:.1rem 0 .08rem .2rem;margin-bottom: .1rem;
        h4{
             line-height: .8rem; font-size: .26rem; color: #333; display: flex;  align-items: center;
            font-weight: normal; justify-content: space-between; box-sizing: border-box; padding-right: .2rem;
            p{
                display: flex; align-items: center;
            }
            i{
                color: #368ef0; font-size: .35rem; margin-right: .1rem;
            }
            img{
                width: .32rem; margin-right: .1rem;
            }
            .red{
                color: #f36363; font-size: .24rem;
            }
        }
        .content{
            box-sizing: border-box; padding: 0 .2rem 0 0;
            p{
                justify-content: flex-end; align-items: center; height: .6rem;font-size: .24rem; display: flex;
                span:first-child{
                    font-size: .24rem;font-weight: bold;  color: #333; margin-right: .5rem;
                }
                span:nth-child(2){
                    font-size: .28rem;font-weight: bold;  color: #f42434;
                }
            }
        }
        ul{
            padding: .1rem 0 ; border-bottom: 1px dashed #cdcccc;
            li{
                line-height: .5rem; font-size: .24rem; color: #666;
            }
        }
    }
</style>