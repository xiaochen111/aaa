<template>
    <div class="indexmain">
        <div class="title-top">
            <span>{{$t('home.group')}}</span>
        </div>
        <tipload></tipload>
        <scroll  class="recommend-content" :data="scrollList" @tipsPosDef="tipsPosDef" :listenScroll='true' >
            <div>
                <van-swipe :autoplay="3000" class="swipe">
                    <van-swipe-item v-for="(item,index) in bannerList" :key="index"><img :src="require('@/asset/image/index/'+ item+language +'.jpg')" alt="" srcset=""></van-swipe-item>
                </van-swipe>
                <div class="module">
                    <h4>
                        <span class="tit">{{$t('home.fcl')}}</span>
                        <router-link  class="more" to="/enquiry">{{$t('common.more')}} <i class="iconfont icon-jiebantubiao-"></i></router-link>
                    </h4>
                    <ul class="tab-change">
                        <li :class="tabnum === 0 ? 'act':''" @click="changeTab(0)"><i class="iconfont icon-webcangdanhaiyun line"></i> <span>{{$t('home.hotroute')}}</span></li>
                        <li :class="tabnum === 1 ? 'act':''" @click="changeTab(1)"><i class="iconfont icon-gangkouboweiweihu port"></i> <span>{{$t('home.hotport')}}</span></li>
                    </ul>
                    <div class="card-content" v-if="tabnum === 0">
                        <div class="card" v-for="(item,index) in portline.routeHotList" :key="index" @click="fcllistlink({routeIdsStr:item.routeId,name:item['routeIdName'+language]})">
                            <img :src="require('@/asset/image/index/router/'+ item.routeId +'.jpg')" alt="" srcset="" width="100%">
                            <!-- <img :src="`./static/images/router/${item.routeIdNameEn}.jpg`" alt="" srcset="" width="100%"> -->
                            <div class="content">
                                <p class="num">{{item.freightFclCnt}}{{$t('home.items')}}</p>
                                <p class="space">{{item['routeIdName'+language]}}</p>
                                <p class="intro">{{$t('home.pod')}}{{item.portEndCnt}}{{$t('home.pieces')}}</p>
                            </div>
                        </div>
                    </div>
                    <div class="card-content" v-if="tabnum === 1">
                        <div class="card" v-for="(item,index) in portline.portHotList" :key="index" @click="fcllistlink({portStartId:item.portStartId,name:item['portStartIdName'+language]})">
                            <img :src="require('@/asset/image/index/port/'+ item.portCode +'.jpg')" alt="" srcset="" width="100%">
                            <!-- <img :src="`./static/images/port/${item.portCode}.jpg`" alt="" srcset="" width="100%"> -->
                            <div class="content">
                                <p class="num">{{item.portStartCnt}}{{$t('home.items')}}</p>
                                <p class="space">{{item['portStartIdName'+language]}}</p>
                                <p class="intro">{{$t('home.pod')}}{{item.portEndCnt}} {{$t('home.pieces')}}</p>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="module">
                    <h4>
                        <span class="tit">{{$t('home.searvice')}}</span>
                         <router-link  class="more" to="/scheme">{{$t('common.more')}} <i class="iconfont icon-jiebantubiao-"></i></router-link>
                    </h4>
                    <ul class="service">
                        <li @click="toDeatil('sea')">
                            <img width="100%"  v-lazy="require('@/asset/image/index/airfreight.jpg')" alt="" srcset="">
                            <span>{{$t('home.sea')}}</span>
                        </li>
                        <li @click="toDeatil('air')">
                            <img width="100%" v-lazy="require('@/asset/image/index/ITservice.jpg')"   alt="" srcset="">
                            <span>{{$t('home.air')}}</span>
                        </li>
                        <li @click="toDeatil('jiejuefangan')">
                            <img width="100%" v-lazy="require('@/asset/image/index/projectservice.jpg')"  alt="" srcset="">
                            <span>{{$t('home.it')}}</span>
                        </li>
                        <li @click="toDeatil('xiangmuwuliu')">
                            <img width="100%" v-lazy="require('@/asset/image/index/seafreight.jpg')"   alt="" srcset="">
                            <span>{{$t('home.project')}}</span>
                        </li>
                    </ul>
                </div>
                <div class="module">
                    <h4>
                        <span class="tit">{{$t('aboutus.aboutus')}}</span>
                        <router-link  class="more" to="/aboutus">{{$t('common.more')}} <i class="iconfont icon-jiebantubiao-"></i></router-link>
                    </h4>
                    <ul class="aboutus">
                        <li>
                            <router-link to="/introhs">
                                <img v-lazy="require('@/asset/image/index/guanyuhuanshi.jpg')"  alt="" srcset="">
                                <span>{{$t('aboutus.Introduction')}}</span>
                            </router-link>
                        </li>
                        <li>
                            <router-link to="/fwzz">
                                <img v-lazy="require('@/asset/image/index/zongzhifuwu.jpg')"  alt="" srcset="">
                                <span>{{$t('aboutus.serviceTenet')}}</span>
                            </router-link>
                        </li>
                        <li>
                            <router-link to="/fzzl">
                                <img v-lazy="require('@/asset/image/index/fazhanzhanlue.jpg')"  alt="" srcset="">
                                <span>{{$t('aboutus.develop')}}</span>
                            </router-link>
                        </li>
                    </ul>
                </div>
                <div class="module">
                    <h4>
                        <span class="tit">{{$t('home.News')}}</span>
                        <router-link  class="more" to="/newslist">{{$t('common.more')}} <i class="iconfont icon-jiebantubiao-"></i></router-link>
                    </h4>
                    <ul class="news">
                        <li v-for="(item,index) in newlist" :key="index" @click="toNewsDetail(item.id)">
                            <div class="tip-txt">{{$t('home.CompanyNews')}}</div>
                            <img v-lazy="aliyuncs+item.picPath" alt="" srcset="" :key="aliyuncs+item.picPath">
                            <p>{{item.title}}</p>
                        </li>
                    </ul>
                </div>
                <div class="module">
                    <h4>
                        <span class="tit">{{$t('home.MarineTools')}}</span>
                        <router-link  class="more" to="/tools">{{$t('common.more')}} <i class="iconfont icon-jiebantubiao-"></i></router-link>
                    </h4>
                    <ul class="tools">
                        <li @click="linkTo('http://www.wlog.ltd/air/index.html')">
                            <img v-lazy="require('@/asset/image/index/airsearch.svg')" alt="" srcset="" >
                            <p>{{$t('home.AirBookingSearch')}}</p>
                        </li>
                        <li @click="linkTo('http://116.228.182.6:8088/hsky/faces/admin/login.jsp')" >
                             <img v-lazy="require('@/asset/image/index/air.svg')" alt="" srcset="">
                             <p>{{$t('home.AirStoreSearch')}}</p>
                        </li>
                        <li>
                             <img v-lazy="require('@/asset/image/index/fcl.svg')"  alt="" srcset="" >
                             <p>{{$t('home.SeaTracking')}}</p>
                        </li>
                        <li @click="linkTo('/website/web/schedule/list.html')">
                             <img v-lazy="require('@/asset/image/index/boat.svg')"  alt="" srcset="">
                             <p>{{$t('home.ScheduleSearch')}}</p>
                        </li>
                    </ul>
                </div>
            </div>
        </scroll>
        <div class="bottom-nav">
            <router-link to="/index" class="act">
                 <i class="iconfont icon-shouye"></i>
                <span>{{$t("common.home")}}</span>
            </router-link>
            <router-link  :to="loginflag.flag?'/platform':'/login'">
                <i class="iconfont icon-gongzuotai"></i>
                <span>{{$t("common.Jobplatform")}}</span> 
            </router-link>
            <a v-if="!inapp" href="http://www.wwlcargo.com/website/web/index.html">
                <i class="iconfont icon-pcguanwang"></i>
                <span>{{$t("home.pcPortal")}}</span>
            </a>
        </div>
        <telbox></telbox>
        <!-- <div class="indexfinger" v-if="pickstyle" @click="hidePickerStyleDef">
            <img src="@/asset/image/other/indexfinger.png" alt="" srcset="" class="zhishi" >
        </div> -->
    </div>
</template>

<script>
    import { Swipe, SwipeItem } from "vant";
    import Scroll from "@/components/common/scroll";
    import { mapState, mapMutations, mapGetters } from "vuex";
    import {newsqueryPage,HotPortCntAndRouteCnt} from '@/api/getData'
    import telbox  from '@/components/common/tel'
    import tipload  from '@/components/common/tipload'
    import {aliyuncs} from '@/utils/config'
    import {toDeatil} from '@/utils/common'
    import storage from '@/utils/storage'
    export default {
        components: {
            [Swipe.name]: Swipe,
            [SwipeItem.name]: SwipeItem,
            Scroll,
            telbox,
            tipload,
        },   
        data() {
            return {
                scrollList:[],
                newlist:[],
                aliyuncs:aliyuncs, //阿里云地址
                portline:{},
                tabnum:0,
                bannerList:['fazhanzhanlue','gongxianggongying','quanmiangaiban','yidongduanyoushi'],
                pickstyle:false,
            }
        },
        methods: {
            ...mapMutations(["navTabNumSet","resetCompareFcl","noKeepAlive"]),
            hidePickerStyleDef(){
                this.pickstyle = false;
                let fingerStyle = storage.getObject('fingerStyle')||{};
                fingerStyle.indexfinger = true
                storage.setObject('fingerStyle',fingerStyle)
            },
            newlistCn(){ //获取新闻list
                let num = this.language === 'Cn'?2:1
                newsqueryPage({englishFlag:num}).then(res=>{
                    this.newlist = res.data.slice(0,4);
                    this.scrollList.push(1)
                })
            },
            linkTo(url){
                location.href = url;
            },
            toNewsDetail(id){
                this.$router.push({
                    path:'/newsDetail',
                    query:{id:id},//传参数
                })
            },
            toDeatil,//行业方案跳转方法
            gethotLinePort(){
                HotPortCntAndRouteCnt().then(res=>{
                    let {portHotList,routeHotList} = res.data;
                    portHotList = portHotList.slice(0,4);
                    routeHotList = routeHotList.slice(0,4);
                    this.portline = {portHotList,routeHotList}
                    this.scrollList.push(1)
                })
            },
            changeTab(num){
                this.tabnum = num;
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
        created() {
            this.gethotLinePort();
            this.resetCompareFcl();
            this.noKeepAlive('fcllist');
            let fingerStyle = storage.getObject('fingerStyle')||{};
            if(!fingerStyle.indexfinger){
                this.pickstyle = true
            }
            this.$on('scroll',(pos)=>{
                console.log(pos)
            })
        },
        mounted() {
            this.newlistCn();
            
        },
        computed:{
            ...mapState(['language','loginflag']),
        },
        watch: {
            language(){
                this.newlistCn()
            }
        },
    }
</script>

<style lang="less" scoped>
    .indexmain{
        height:100%;overflow: hidden; box-sizing: border-box; padding-top: .85rem; padding-bottom: .85rem;
    }
    .tipPoswidth{
        position: fixed; left: 0; top: 50px; width: 100%; z-index: 99;
    }
    .indexfinger{
        height: 100%; width: 100%; position: fixed; left: 0; top: 0; background: rgba(0, 0, 0, .5);z-index: 999;
    }
    .zhishi{
        width: 3.5rem;top: 240px; right: 10px; position: absolute;
    }
    .swipe {
        height: 2.78rem;
        img {
            height: 2.78rem;
        }
    }
    .module{
        background: #fff; box-sizing: border-box; padding: .2rem .3rem .1rem; margin-top: .2rem;
        h4{
            box-sizing: border-box; padding-bottom: .2rem; display: flex; justify-content: space-between; align-items: center; font-weight: normal;
            .tit{
                font-size: .3rem; color: #333;
            }
            .more{
                font-size: .24rem; color: #666;
                i.iconfont{
                    font-size: 10px; color: #2f94f1;
                }
            }
        }
        .tab-change{
            display: flex; align-items: center; align-items: center; justify-content: space-around; font-size: .28rem;
            li{
              display: flex; align-items: center; align-items: center; color: #666; font-size: .26rem; 
              &.act{
                  color: #333;font-size: .3rem;
              }
            }
            .line{
                color: #39c7e2;
            }
            .port{
                color: #ff7c5f;font-size: 24px;
            }
            i{
                font-size: 22px; margin-right: 5px;  margin-top: 3px;
            }
        }
        .card-content{
            display: flex;flex-wrap:wrap; justify-content: space-between; box-sizing: border-box; padding-top: .2rem;
            .card{
                width: calc(~'50% - .1rem');  box-shadow: 0 0 10px 0px rgba(237, 238, 240, 0.85); margin-bottom: .2rem;font-size: 0;
                .content{
                    box-sizing: border-box;  padding: .15rem .2rem; 
                    p{
                        font-size: .28rem; box-sizing: border-box; padding-bottom: .02rem;
                    }
                    .num{
                        font-size: .28rem; color: #f36363; margin-bottom: .1rem;;
                    }
                    .space{
                        font-size: .24rem; color: #333;
                    }
                    .intro{
                        font-size: .2rem; color: #949494;
                    }
                }
            }
        }
    }
    .service{
        display: flex; justify-content: space-between;
        li{
            width: 24%; position: relative;
            span{
                position: absolute; top: .1rem; left: .1rem; font-size: .26rem; color: #666;
            }
        }
    }
    .aboutus{
        display: flex; justify-content: space-between;
        li{
            width: 30%; 
            a{
               display: flex; align-items: center; flex-direction: column; width: 100%; height: 100%;
            }
            img{
                width: 100%;
            }
            span{
                color: #666; font-size: .28rem; line-height: .5rem;
            }
        }
    }
    .news{
        display: flex;flex-wrap:wrap; justify-content: space-between;
        li{
            width: 48%;  height: 100px; margin-bottom: .15rem; position: relative;
            img{
                height: 100%; width: 100%;
            }
            .tip-txt{
                height:.5rem; line-height:.5rem; box-sizing: border-box; padding: 0 .15rem; background-image: linear-gradient(180deg, #57AAFA 0%, #2D8DE2 50%, #056FCA 100%);
                border-radius: 0 0 .25rem 0; position: absolute; left: 0; top: 0; color: #fff; font-size: .2rem;
            }
            p{
                width: 100%; line-height: .5rem; position: absolute; left: 0; bottom: 0; font-size: .2rem; box-sizing: border-box; padding: 0 .15rem;
                overflow: hidden;text-overflow: ellipsis; white-space: nowrap; color: #fff;
            }
        }
    }
    .tools{
        display: flex;flex-wrap:wrap; justify-content: space-between;
        li{
            width: 48%;  margin-bottom: .15rem; border-radius: 5px;text-align: center; font-size: 0;
            img{
                width: 60%;
            }
            p{
                line-height: .55rem; height: .55rem; font-size: .24rem; text-align: center; color: #666;
                border-top: 1px dashed #7eb4e3; margin-top: .1rem;
            }
            &:nth-child(1){
                background: #e5eef7;
            }
            &:nth-child(2){
                background: #f1d7bd;
                p{
                    border-top: 1px dashed #f3b576;
                }
            }
            &:nth-child(3){
                background: #c1d7ec;
            }
            &:nth-child(4){
                background: #e5f5f7;
                p{
                    border-top: 1px dashed #f3b576;
                }
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
        height: .85rem; color: #fff;  box-sizing: border-box; padding: 0 .2rem; display: flex; justify-content: space-around; top: 0;
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