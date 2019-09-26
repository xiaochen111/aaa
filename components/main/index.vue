<template>
    <div style="height:100%">
        <!-- <router-view class="router"></router-view> -->
        
        <!-- <keep-alive>
            <router-view class="router" v-if="$route.meta.keepAlive"></router-view>
        </keep-alive>
        <router-view class="router" v-if="!$route.meta.keepAlive"></router-view> -->
        
        <van-tabbar v-model="navTab.active">
            <van-tabbar-item to="/index">
                <span>{{$t("common.home")}}</span>
                <template slot="icon" slot-scope="props">
                <i class="iconfont icon-shouye"></i>
                </template>
            </van-tabbar-item>
             <van-tabbar-item to="/enquiry">
                <!-- <span v-if="fob">FOB</span> -->
                <span>{{$t("common.freight")}}</span>
                <template slot="icon" slot-scope="props">
                <i class="iconfont icon-xunjia1"></i>
                </template>
            </van-tabbar-item>
            <van-tabbar-item to="/newslist">
                <span>{{$t("common.news")}}</span>
                <template slot="icon" slot-scope="props">
                <i class="iconfont icon-zixun"></i>
                </template>
            </van-tabbar-item>
            <van-tabbar-item to="/menber">
                <span>{{$t("common.member")}}</span>
                <template slot="icon" slot-scope="props">
                <i class="iconfont icon-huiyuanzhongxin"></i>
                </template>
            </van-tabbar-item>
        </van-tabbar>

        <ul class="fixed-ul">
            <li @click="wwlconsultFn">
                <a>
                <!-- <a :href="'tel:'+tel['tel'+language]"> -->
                    <img src="../../../static/images/index/lianxiwomen.png" alt="">
                </a>
            </li>
            <li @click="languageChange">
                <img :src="`./static/images/index/${languageImg}.png`" alt="">
            </li>
        </ul>

        <van-popup v-model="telshow">
            <telbox></telbox>
        </van-popup>
    </div>
</template>

<script>
    import { Tabbar, TabbarItem ,Popup } from 'vant';
    import {mapState, mapMutations, mapGetters} from 'vuex'
    import {wwlconsult} from '@/api/getData'
    import storage from '@/utils/storage'
    import telbox  from '@/components/common/tel'
    import {permissionsDef} from '@/utils/common'
    export default {
        components:{
            [Tabbar.name]:Tabbar,
            [TabbarItem.name]:TabbarItem,
            [Popup.name]:Popup,
            telbox,
        },
        data(){
            return {
                languageImg:'yingwen',
                show : true,
                telshow:false,
                fob:false,
            }
        },
        created() {
            let permissions =  permissionsDef();
            if(permissions['/web/freight/freightfclweb/queryFreightFclListForFob.html']){
                this.fob = true
            }
        },
        mounted(){
            console.log(this.navTab)
        },
        computed:{
            ...mapState(['navTab','tel','language']),
        },
        methods:{
            ...mapMutations(['setLanguage']),
            wwlconsultFn(){
                this.telshow = true;
                wwlconsult();
            },
            languageChange(){
                let locale = this.$i18n.locale
                locale === 'zh' ? this.$i18n.locale = 'en' : this.$i18n.locale = 'zh'

                if(locale === 'zh'){
                    this.setLanguage('En')
                    // storage.setValuelocal('language','En')
                    this.languageImg ="zhongyinwen"
                }else{
                    this.setLanguage('Cn')
                    // storage.setValuelocal('language','Cn')
                    this.languageImg ="yingwen"
                }
            },

        }
    }
</script>

<style lang='less' scoped>
    i{
        font-size: .45rem; 
    }
    .van-popup{
        width: 80%;
    }
    .router{
        padding-bottom: 50px;
    }
    .fixed-ul{
        position: fixed; width: 1.16rem; height:1.14*2rem; top: 50%;margin-top: -1.14rem; right: 0;
        li{
            width: 1.16rem; height:1.14rem;
            img{
                width: 1.16rem; height:1.14rem;
            }
        }
    }
</style>