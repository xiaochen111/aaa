<template>
    <div id="message" @click="showmsgflag=false">
        <nav-bar :title="$t('home.messageCenter')">
            <div class="show-msg">
               <van-icon name="wap-nav" @click.stop="showmsgflag= !showmsgflag"/>
               <div class="msg-box" v-if="showmsgflag" @click.stop="">
                   <p @click.stop="readDef"><i class="iconfont icon-yidu"></i> {{$t('common.allread')}}</p>
               </div>
            </div>
        </nav-bar>

        <div>
            <form action="/">
                <van-search
                    v-model="value"
                    :placeholder="$t('common.searchkeywords')"
                    @search="onSearch"
                    background="#fff"
                    @clear-callback="clearfn"
                />
            </form>
        </div>

        
        <div class="content-box">
            <ul class="tab">
                <li @click="tabType(2)" :class="type==2?'act':''"><i class="iconfont icon-dingdan"></i> <span > {{$t('common.Ordertracking')}} </span></li>
                <li @click="tabType(1)" :class="type==1?'act':''"><i class="iconfont icon-xitongxiaoxi"></i> <span > {{$t('common.systemMsg')}}</span></li>
            </ul>
            <scroll ref="scroll" class="recommend-content" :data="list" :scrollx=false :pullup=true @scrollToEnd="onLoad">
                <div class="msg-box">
                    <ul>
                        <van-cell-swipe :right-width="65"  v-for="(item,index) in list" :key="index" >
                            <li class="border-bottom">
                                <div class="tip-notice">
                                    <span class="weidu" v-if="item.status == 2"></span>
                                    <i class="iconfont icon-tongzhi"></i>
                                </div>
                                <div class="content-li">
                                    <p class="tit">{{item.title}}</p>
                                    <div class="sm-detail" v-html="item.content" @click="readed($event,item.id)">
                                    </div>
                                    <p class="date">{{item.updateTime | dateInit('MM-DD-YYYY',language)}}</p>
                                </div>
                            </li>
                            <div slot="right" class="delete" >
                                <div class="delete-box" @click="deleteDef(item.id,index)">
                                    <i class="iconfont icon-shanchu"></i>
                                </div>
                            </div>
                        </van-cell-swipe>
                        <p class="loading" v-show="loading">{{loadTxt||$t('common.loading')}}</p>
                        <p class="loading" v-show="!loading">{{loadTxt||$t('common.loading')}}</p>
                    </ul>
                </div>
            </scroll>
        </div>

        <div class="tab-fiexd">
            <div :class="status==1?'tab-filter tab-act':'tab-filter'" @click="allReadedDef(1)">{{$t('common.Alreadyread')}}</div>
            <div :class="status==2?'tab-filter tab-act':'tab-filter'" @click="allReadedDef(2)">{{$t('common.Unread')}}</div>
        </div>
        
    </div>
</template>

<script>
    import Scroll from '@/components/common/scroll'
    import navBar from '@/components/common/navBar'
    import { Icon ,Search,CellSwipe ,Toast} from 'vant';
    import { message } from '@/api/getData';
    import {mapState,mapMutations, mapGetters} from 'vuex'
    export default {
        name:'message',
        components:{
            [Icon.name]:Icon,
            [CellSwipe.name]:CellSwipe,
            [Search.name]:Search,
            [Toast.name]:Toast,
            navBar,
            Scroll
        },
        data() {
            return {
                showmsgflag:false,
                value:'',
                type:2,
                status:'',
                params:{
                    pageNo:1,
                },
                list:[],
                loading: false,
                loadTxt:'',
                finished:false, //少于10  不再请求数据
            }
        },
        created() {
            this.getDate(this.params);
        },
        methods: {
            ...mapMutations(['keepAlivedef']),
            clearfn(){
               this.onSearch();
            },
            onSearch(){
                this.params = {
                    title : this.value,
                    pageNo : 1
                }
                this.list = [];
                this.getDate(this.params);
            },
            allReadedDef(status){
                this.status = status
                this.resetParmes();
                this.getDate(this.params);
            },
            getDate(params){
                Object.assign(params,{type:this.type,status:this.status})  //type 1=系统消息  2=订单跟踪
                message('queryMessageNotice',params).then(res=>{
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
            readDef(){
                message('updateAllSysMessageNoticeRead',{}).then(res=>{
                    if(res.data.code == '1'){
                        this.showmsgflag = false;
                        this.list.forEach(item => {
                            item.status = 1;
                        });
                    }
                })
            },
            toDetail(id){
                this.$router.push({
                    path:'/messagedetail',
                    query:{id},//传参数
                })
            },
            tabType(num){
               this.resetParmes(num);
               this.getDate(this.params);
            },
            resetParmes(type){
                let defaultType = this.type
                this.type = type || defaultType;
                this.list = [];
                this.loading = false;
                this.loadTxt = '';
                this.finished = false;
                this.params.pageNo = 1;
            },
            deleteDef(ids,index){
                message('deleteMessageNoticeByIds',{ids}).then(res=>{
                    if(res.data.code == '1'){
                        this.list.splice(index,1);
                        Toast('删除成功')
                    }
                })
            },
            readed(e,ids){
                let htrmlStr = e.toElement.localName == 'a'?e.target.outerHTML:e.target.innerHTML;
                let linkStr = htrmlStr.match(/#\/myorderDetail\?id=\d+/gi)[0];
                // location.href = linkStr;
                let id = linkStr.match(/\d+/g)[0];
                this.$router.push({
                    path:'myorderDetail',
                    query:{id}
                })
                message('updateSysMessageNoticeRead',{ids})
            }
        },
        computed:{
            ...mapState(['language']),  //这个是后台中英文翻译的关键
        },                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 }
</script>

<style lang="less" scoped>
    #message{
       height: 100%; overflow: hidden; box-sizing: border-box; padding-top: 46px;
    }
    form{
        margin-bottom: .05rem; 
    }
    .recommend-content{
        height: calc(~"100% - 44px - 45px");
    }
    .tab-fiexd{
        position: fixed; bottom: 0; left: 0; height: 45px; background: #fff;width: 100%;box-shadow: 0 -10px 13px -10px rgba(0, 0, 0, 0.35); 
        display: flex; align-items: center;
        .tab-filter{
            flex-grow:1; display: flex; justify-content: space-around; align-items: center; color: #333; font-size: .28rem;
            &.tab-act{
                color: #368ef0;
            }
        }
    }
    .show-msg{
        position: relative;
        .msg-box{
            background: #fff; position: absolute; right: -11px; top: 40px; z-index: 99; border-radius: 3px;
            box-sizing: border-box; padding: .1rem .2rem;
            p{
                line-height: .4rem; font-size: .22rem; color: #666; display: flex; align-items: center;white-space:nowrap;
                i{
                    margin-right:7px;
                }
            }
            &:after{
                width: 0;height: 0;border-left: 8px solid transparent;border-right: 8px solid transparent;border-bottom: 12px solid #fff;
                content: ''; display: block; position: absolute; right: 10px; top: -12px;
            }
        }
    }
    .content-box{
        background: #fff; margin-top: .1rem; box-sizing: border-box; padding: 0 .2rem;height: calc(~"100% - .7rem"); overflow: hidden;
        .tab{
            display: flex; box-sizing: border-box; padding: .1rem 0;
            li{
                flex-grow:1; text-align: center; font-size: .26rem; line-height: .5rem; color: #333;
                &.act{
                    color: #368ef0;
                    span{
                         border-bottom: 1px solid #368ef0;
                    }
                }
            }
        }
        .msg-box{
            margin-top: .1rem; box-sizing: border-box; padding-bottom: .2rem;
            li{
                box-sizing: border-box; padding: .2rem 0; display: flex;
                .tip-notice{
                    width: .82rem; height: .82rem; border-radius: 3px; background: #f59268;flex-shrink:0;
                    display: flex; align-items: center; justify-content: space-around; position: relative;
                    i{
                        font-size: 25px; color: #fff;
                    }
                    .weidu{
                        width: 10px;height: 10px;border-radius: 50%; background: red; position: absolute; right: -4px; top: -4px;
                    }
                }
                .content-li{
                    margin-left: .15rem; width: calc(~"100% - .95rem");
                    .tit{
                        font-size: .26rem; color: #333; overflow: hidden;text-overflow: ellipsis; white-space: nowrap; width: 100%; margin-bottom: .15rem;
                    }
                    .sm-detail{
                        box-sizing: border-box; padding: .1rem; background: #e5e5e5; color: #949494; line-height: .4rem; font-size: .22rem; border-radius: 3px;
                         margin: .1rem 0;
                    }
                    .date{
                        line-height: .4rem; font-size: .22rem; color: #949494; 
                    }
                }
            }
        }
    }
    .loading{
        line-height: .44rem; font-size: .18rem; text-align: center;color: #666;
    }
    
</style>