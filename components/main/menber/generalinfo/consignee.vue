<template>
    <div id="shipper">
        <nav-bar :title="$t('common.Consignee')">
            <router-link to="/consigneeedit">{{$t('common.add')}}</router-link>
        </nav-bar>
        <form action="/">
            <van-search
                v-model="value"
                :placeholder="$t('common.searchkeywords')"
                @search="onSearch"
                background="#fff"
            />
        </form>
        <scroll class="recommend-content" :data="list" :pullup=true @scrollToEnd="onLoad">
            <ul class="content-main">
                <van-cell-swipe :right-width="75" v-for="(item,index) in list" :key="index">
                    <li class="item">
                        <p class="name">{{item.name}}</p>
                        <div class="item-main">
                            <img :src="item.pathCountryLogo" alt="" srcset="">
                            <div class="right-content">
                                <div class="top-t">
                                    <p class="left">
                                        <span class="contacter">{{item.contact}}</span>
                                        <span class="tel">{{item.tel}}</span>
                                        <span class="moren" v-if="item.isDefault == 1">默认</span>
                                    </p>
                                    <router-link :to="`/consigneeedit?id=${item.id}`">
                                        <i class="iconfont icon-bianjixiugai"></i>
                                    </router-link>
                                </div>
                                <div class="address">
                                    {{item.address}}
                                </div>
                                <p class="email-code">
                                    <span>{{item.eMail}}</span>
                                    <span>{{item.postalCode}}</span>
                                </p>
                            </div>
                        </div>
                    </li>
                    <div slot="right" class="delete" @click="deleteIds(item.id)">
                        <div class="delete-box">
                            <i class="iconfont icon-shanchu"></i>
                        </div>
                    </div>
                </van-cell-swipe>
                <p class="loading" v-show="loading">{{loadTxt||$t('common.loading')}}</p>
                <p class="loading" v-show="!loading">{{loadTxt||$t('common.loading')}}</p>
            </ul>
        </scroll>
    </div>
</template>

<script>
    import navBar from '@/components/common/navBar'
    import Scroll from "@/components/common/scroll"
    import { Search,CellSwipe } from 'vant'
    import {consignee} from '@/api/getData'
    import { mapState, mapMutations, mapGetters } from "vuex";
    export default {
        components:{
            navBar,Scroll,
            [Search.name]:Search,
            [CellSwipe.name]:CellSwipe,
        },
        data() {
            return {
                value:'',
                params:{
                    pageNo:1
                },
                list:[],
                loading: false,
                loadTxt:'',
                finished:false, //少于10  不再请求数据
            }
        },
        created() {
            this.getDate(this.params)
        },
        mounted() {
        },
        computed:{
            ...mapState(['language']),
        },
        methods: {
            onSearch(){
                this.resetParamDef();
                this.params.name= this.value;
                this.getDate(this.params)
            },
            getDate(params){
                consignee('queryPage',params).then(res=>{
                    this.params.pageNo++;
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
            onLoad(){
                this.loading = true;
                this.loadTxt = this.language=="Cn"?'加载中。。':'loading..';
                if(this.finished){
                    this.loading = false;
                    this.loadTxt = this.language=="Cn"? '数据全部加载完成':'The data is fully loaded';
                    return
                }
                 this.getDate(this.params)
            },
            deleteIds(ids){
                consignee('deleteByIds',{ids}).then(res=>{
                    if(res.data.code==1){
                        this.params.pageNo = 1;
                        this.list = [];
                        this.getDate(this.params)
                    }
                })
            },
            resetParamDef(){
                this.params = {pageNo:1};
                this.list = [];
                this.loading = false;
                this.loadTxt = '';
                this.finished = false;
            },
        },
    }
</script>

<style lang="less" scoped>
    #shipper{
        box-sizing: border-box; padding-top: 46px; height: 100%; overflow: hidden;
    }
    .van-nav-bar__right{
        a{
            color: #fff;
        }
    }
    form{
        margin-bottom: .05rem; 
    }
    .recommend-content{
        height: calc(~"100% - 44px");
    }
    .content-main{
        box-sizing: border-box;  background: #fff; padding:.1rem 0 0.1rem .2rem;
        .item{
            border-bottom: 1px dashed #cdcccc; padding-bottom: .1rem;
            .name{
                line-height: .4rem; font-size: .26rem; color: #333; padding: .08rem 0;
            }
        }
        .item-main{
            display: flex;
            img{
                width: .6rem; flex-shrink:0; height: .6rem; margin-right: .2rem;border-radius: 5px;
            }
            .right-content{
                width: 100%; margin-right: .1rem;
            }
            .top-t{
                display: flex; justify-content: space-between; align-items: center;
                a{
                    font-size: 16px; color: #666;
                }
                i{
                    font-size: 22px;
                }
            }
            .left{
                display: flex; align-items: center;
            }
            .contacter,.tel{
                font-size: .32rem; color: #333; margin-right: 10px;
            }
            .tel{
                font-size: .24rem;
            }
            .moren{
                display: inline-block; font-size: .18rem; color: #f42434; box-sizing: border-box; padding: .02rem .08rem; background: #e9c5c5;
                border-radius: 3px;
            }
            .address,.email-code{
                padding: .1rem 0;  font-size: .22rem; line-height: .3rem; color: #666;
            }
            .email-code{
                padding-top: .051rem;
                span{
                    margin-right: 20px;
                }
            }
        }
    }
    .loading{
        line-height: .44rem; font-size: .18rem; text-align: center;color: #666;
    }
</style>