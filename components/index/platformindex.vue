<template>
    <div id="platformindex">
        <div class="title-top">
            <!-- <a></a> -->
            <!-- <i :class="language==='En'?'iconfont icon-shouye-zhong':'iconfont icon-shouye-yingwen'" @click="languageChange"></i> -->
            <span>{{$t('home.group')}}</span>
            <!-- <router-link to="/message">
                <i class="iconfont icon-youxiang1"></i>
                <a v-if="msgNum" class="msgNum">{{msgNum}}</a>
            </router-link> -->
        </div>
        <router-view></router-view>
        <div class="bottom-nav">
            <router-link to="/platformindex" :class="navTab.active==0?'act':''">
                <i class="iconfont icon-shouye"></i>
                <span>{{$t("common.Jobplatform")}}</span>
            </router-link>
            <router-link to="/accout" :class="navTab.active==2?'act':''">
                <i class="iconfont icon-huiyuanzhongxin"></i>
                <span>{{$t("common.me")}}</span>
            </router-link>
            <a v-if="dev" :class="navTab.active==3?'act':''" href="http://192.168.50.235:8022/?a=1">
                <i class="iconfont icon-IM-duihua"></i>
                <span>IM</span> 
            </a>
            <a v-else :class="navTab.active==3?'act':''" href="/admin/im/mobileim/index.html?a=1">
                <i class="iconfont icon-IM-duihua"></i>
                <span>IM</span> 
            </a>
        </div>
    </div>
</template>

<script>
    import {mapState, mapMutations} from 'vuex'
    import { message,mobileImCheck } from '@/api/getData';
    import {setCookie} from '@/utils/common'
    import {permissionsDef} from '@/utils/common'
    export default {
        computed:{
            ...mapState(['navTab','language']),
        },
        data() {
            return {
                msgNum:'',
                dev:true,
            }
        },
        created() {
            this.dev = process.env.NODE_ENV == 'development'
        },
        methods: {
            ...mapMutations(['setLanguage']), //触发动态数据的语言切换
            languageChange(){
                let locale = this.$i18n.locale
                if(locale === 'zh'){
                    this.$i18n.locale = 'en'
                    this.setLanguage('En')
                    setCookie('language_website','en')
                }else{
                    this.$i18n.locale = 'zh'
                    this.setLanguage('Cn')
                    setCookie('language_website','cn')
                }
            },
        },
    }
</script>

<style lang="less" scoped>
    #platformindex{
        height:100%;overflow: hidden; box-sizing: border-box; padding-top: .85rem; padding-bottom: .85rem;
    }
    .bottom-nav{
        position: fixed; width: 100%; bottom: 0; left: 0; background: #fff; height: .85rem;
        box-shadow: 0 -10px 13px -10px rgba(0, 0, 0, 0.35); display: flex; justify-content: space-around; align-items: center;
        font-size: .22rem; color: #666;
        a{
            display: flex; flex-direction: column; align-items: center;color: #666; flex: 1;
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
            font-size: 23px; color: #fff;
        }
        a{
            font-size: 22px; color: #fff; display: flex; flex-direction: column; align-items: center; width: .5rem; position: relative;
            .msgNum{
                display: block; height: 16px; min-width: 12px; background:red; color: #fff; position: absolute; top: -5px; left: 100%; transform: translateX(-50%); font-size: 12px;
                border-radius: 16px; text-align: center; line-height: 16px; width: auto; padding: 0 2px;
            }
        }
        span{
            font-size: .24rem;flex-shrink:1;width: 3.8rem; text-align: center;
        }
    }
</style>