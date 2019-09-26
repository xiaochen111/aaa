<template>
    <div id="accout">
        <router-link class="user" to="/card">
             <div class="main-t">
                <img :src="!(new RegExp('null').test(loginflag.infor.headPic))?loginflag.infor.headPic:'./static/images/menber/morentouxiang.png'" alt="" srcset="">
                <span>{{loginflag.infor.nameCn}}</span>
            </div>
            <van-icon name="arrow" class="icon-right" />
        </router-link>

        <ul class="ul-list">
            <li>
                <router-link to="/enquiry">
                    <svg class="icon" aria-hidden="true">
                        <use xlink:href="#icon-baojiadan"></use>
                    </svg>
                    <p class="border-bottom">
                        <span>我的报价</span>
                        <van-icon name="arrow" class="icon-right" />
                    </p>
                </router-link>
            </li>
            <li>
                <router-link to="/myorder">
                    <svg class="icon" aria-hidden="true">
                        <use xlink:href="#icon-wodedingdan-xiaoshou"></use>
                    </svg>
                    <p class="border-bottom">
                        <span>我的订单</span>
                        <van-icon name="arrow" class="icon-right" />
                    </p>
                </router-link>
            </li>
            <!-- <li class="pw">
                <router-link>
                    <svg class="icon" aria-hidden="true">
                        <use xlink:href="#icon-zhongzhimima"></use>
                    </svg>
                    <p class="border-bottom">
                        <span>我的客户</span>
                        <van-icon name="arrow" class="icon-right" />
                    </p>
                </router-link>
            </li> -->
        </ul>
        

        <ul class="ul-list">
            <li class="pw">
                <router-link to="/pwreset">
                    <svg class="icon" aria-hidden="true">
                        <use xlink:href="#icon-zhongzhimima"></use>
                    </svg>
                    <p class="border-bottom">
                        <span>重置密码</span>
                        <van-icon name="arrow" class="icon-right" />
                    </p>
                </router-link>
            </li>
        </ul>


        <ul class="ul-list" v-if="hasIm">
            <li @click="showPick">
                <svg class="icon" aria-hidden="true">
                    <use xlink:href="#icon-gongzuo"></use>
                </svg>
                <p class="border-bottom">
                    <span>工作状态</span>
                    <a>{{state}} <van-icon name="arrow" class="icon-right" /></a>
                </p>
            </li>
        </ul>

        <p class="comfire" v-if="weixin"><button @click="logOutFn">{{$t('setting.binding')}}</button></p>
        <p class="comfire" v-else><button @click="logOutFn">{{$t('setting.exit')}}</button></p>

        <van-picker :columns="columns"  title="标题" show-toolbar v-if="pickshow"
            @cancel="onCancel"
            @confirm="onConfirm" />
    </div>
</template>

<script>
    import { NavBar,Icon,Picker } from 'vant';
    import {logOut} from '@/api/sysapi'
    import storage from '@/utils/storage'
    import {deleteCookie} from '@/utils/common'
    import {projectname} from '@/utils/config'
    import {changeOnlineStatus} from '@/api/getData'
    import {mapMutations,mapState} from 'vuex'
    import {permissionsDef} from '@/utils/common'
    export default {
        components:{
            [NavBar.name]:NavBar,
            [Icon.name]:Icon,
            [Picker.name]:Picker,
        },
        data(){
            return {
                weixin:false,
                columns: ['在线', '忙碌', '离线'],
                pickshow:false,
                state:'在线',
                hasIm:false
            }
        },
         computed:{
            ...mapState(['loginflag']),
        },
        created() {
           this.navTabNumSet(2);
           this.hasIm = permissionsDef('/im/webchat/index.html')
           if(this.isWeiXin()){
               this.weixin = true
           }
        },
        methods:{
            ...mapMutations(['setTel','navTabNumSet']),
            onClickLeft(){
                history.go(-1)
            },
            
            onCancel(){
                this.pickshow = false;
            },
            onConfirm(state, index){
                if(index == 0){
                    index = 1
                }else if(index == 1){
                    index = 4
                }else{
                    index = 6
                }
                changeOnlineStatus({status:index}).then(res=>{
                    if(res.data.code == '1'){
                        this.state = state
                        this.pickshow = false;
                    }
                })
            },
            showPick(){
                this.pickshow = true;
            },
            // 退出
            logOutFn(){
                logOut({}).then(res=>{
                    storage.remove('permissions')
                    deleteCookie(`auth_admin`);
                    this.$router.push('/login');
                })
            },
            isWeiXin(){
                //window.navigator.userAgent属性包含了浏览器类型、版本、操作系统类型、浏览器引擎类型等信息，这个属性可以用来判断浏览器类型
                var ua = window.navigator.userAgent.toLowerCase();
                //通过正则表达式匹配ua中是否含有MicroMessenger字符串
                if(ua.match(/MicroMessenger/i) == 'micromessenger'){
                    return true;
                }else{
                    return false;
                }
            }
        },
        filters:{
            phoneNum(str){
                if(str){
                    var matchstr = str.slice(4,8);
                    var strs = str.replace(matchstr,'****');
                    return strs
                }
            }
        }
    }
</script>

<style lang="less" scoped>
    .user{
        height: 2rem; margin-bottom: .1rem; background: #fff; display: flex; align-items: center;
        box-sizing: border-box; padding: 0 .2rem 0 .4rem; justify-content: space-between;
        .main-t{
            display: flex; align-items: center;
        }
        img{
            width: 1.3rem; height: 1.3rem;
        }
        span{
            font-size: .3rem; margin-left: .4rem; color: #333;
        }
        .icon-right{
            float:right;font-size: .3rem; color: #666;
        }
    }
    .van-picker{
        z-index: 99; position: fixed; width: 100%; bottom:0;
    }
    .ul-list{
        background: #fff;margin: .1rem 0;
        li{
            height:.9rem; padding-left: .2rem; 
            justify-content: space-between; align-items: center;display: flex; 
            img{
                 width: .36rem; height: .36rem;
            }
            svg{
                width: .40rem; height: .40rem;
            }
            p{
                width: 5.7rem;justify-content: space-between; align-items: center;display: flex;
                height:.9rem; font-size: .26rem; padding:0 .2rem; box-sizing: border-box; color: #666;
            }
            .red{
                color: red;
            }
        }
        a{
            display: flex; align-items: center;
        }
        .pw>a{
            width: 100%; justify-content: space-between;
        }
    }

    .comfire{
        box-sizing: border-box; padding: .3rem;font-size: .34rem;margin-top:.3rem;
        button{
            background: #368ef0; color: #fff; height: .7rem; width: 100%; border: none;
            outline: none; border-radius: 4px;
        }
    }
</style>