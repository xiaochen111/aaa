<template>
    <div class="menber">
        <van-nav-bar :title="$t('common.member')" />
        
        <div class="user">
            <img :src="!(new RegExp('null').test(userinfor.headPic))?userinfor.headPic:'./static/images/menber/morentouxiang.png'" alt="" srcset="">
            
            <span>{{userinfor.name}}</span>
        </div>

        <ul class="list-entrance">
            <li>
                <svg class="icon" aria-hidden="true">
                    <use xlink:href="#icon-zhanghushezhi"></use>
                </svg>
                <router-link to="/accout" class="border-bottom"><span>{{$t("common.setting")}}</span><van-icon name="arrow" /></router-link>
            </li>
            <li v-if="myorder">
                <svg class="icon" aria-hidden="true">
                    <use xlink:href="#icon-wodedingdan"></use>
                </svg>
                <router-link to="/myorder" class="border-bottom"><span>{{$t("common.myorder")}}</span><van-icon name="arrow" /></router-link>
            </li>
            <li v-if="advice"> 
                <svg class="icon" aria-hidden="true">
                    <use xlink:href="#icon-wodezhangdan"></use>
                </svg>
                <router-link to="/advice" class="border-bottom"><span>{{$t('common.advice')}}</span><van-icon name="arrow" /></router-link>
            </li>
            <li>
                <svg class="icon" aria-hidden="true">
                    <use xlink:href="#icon-guanyuwomen"></use>
                </svg>
                <router-link to="/aboutus" class="border-bottom"><span>{{$t("common.aboutus")}}</span><van-icon name="arrow" /></router-link>
            </li>
            <li>
                <svg class="icon" aria-hidden="true">
                    <use xlink:href="#icon-changjianwenti"></use>
                </svg>
                <router-link to="/FAQ"><span>{{$t("common.problem")}}</span><van-icon name="arrow" /></router-link>
            </li>
        </ul>
    </div>
</template>

<script>
    import {NavBar,Icon} from 'vant';
    import { mapMutations} from 'vuex'
    import {getUserInfo} from '@/api/getData'
    import {permissionsDef} from '@/utils/common'
    import {projectname} from '@/utils/config'
    export default {
        components:{
            [NavBar.name]:NavBar,
            [Icon.name]:Icon
        },
        data(){
            return {
                userinfor:{},
                myorder:false,
                advice:false,
            }
        },
        created(){
            this.navTabNumSet(3);
            this.getUserInfoFn();
            this.myorder = permissionsDef('/web/webbooking/booking.html');
            this.advice = permissionsDef('/web/webro/.*');
        },
        methods:{
            ...mapMutations(['navTabNumSet','setTel']),

            // 获取个信息
            getUserInfoFn(){
                getUserInfo().then(res=>{
                    this.userinfor = res.data;
                    if(res.data.tel){
                        this.setTel(res.data.tel)
                    }
                })
            }
        }
    }
</script>

<style lang="less" scoped>
    .menber{
        background: #eee;
    }
    .user{
        height: 2rem; margin: .1rem 0; background: #fff; display: flex; align-items: center;
        box-sizing: border-box; padding: 0 .4rem;
        img{
            width: 1.3rem; height: 1.3rem;
        }
        span{
            font-size: .3rem; margin-left: .4rem; color: #333;
        }
    }
    .list-entrance{
        background: #fff;
        li{
            height:.9rem; padding-left: .2rem; 
            justify-content: space-between; align-items: center;display: flex; 
            img{
                width: .36rem; height: .36rem;
            }
            svg{
                width: .45rem; height: .45rem;
            }
            a{
                width: 5.7rem;justify-content: space-between; align-items: center;display: flex;
                height:.9rem; font-size: .26rem; padding:0 .2rem; box-sizing: border-box; color: #666;
            }
        }
    }
</style>