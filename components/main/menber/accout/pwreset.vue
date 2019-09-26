<template>
    <div id="forgetpw">
         <van-nav-bar :title="$t('setting.resetpw')" :left-text="$t('common.goback')" left-arrow fixed  @click-left="onClickLeft">
        </van-nav-bar>

        <div class="ipt-pic">
            <img src="../../../../../static/images/login/pw.png" alt="" srcset="">
            <input :type="seepw.pw3?'password':'text'" :placeholder="$t('setting.oldpw')" v-model="pwd.oldPwd">
            <img src="../../../../../static/images/login/see.png" alt="" srcset="" class="see" @click="seepw.pw3=!seepw.pw3">
        </div>

        <div class="ipt-pic">
            <img src="../../../../../static/images/login/pw.png" alt="" srcset="">
            <input :type="seepw.pw1?'password':'text'" :placeholder="$t('setting.newpw')" v-model="pwd.newPwd">
            <img src="../../../../../static/images/login/see.png" alt="" srcset="" class="see" @click="seepw.pw1=!seepw.pw1">
        </div>

        <div class="ipt-pic">
            <img src="../../../../../static/images/login/pw.png" alt="" srcset="">
            <input :type="seepw.pw2?'password':'text'" :placeholder="$t('setting.comfirepw')" v-model="newPwd1">
            <img src="../../../../../static/images/login/see.png" alt="" srcset="" class="see" @click="seepw.pw2=!seepw.pw2">
        </div>

        <p class="comfire" @click="comfireFn"><button >{{$t('common.confirm')}}</button></p>

        
    </div>
</template>

<script>
    import { NavBar,Toast} from 'vant';
    import { updateSysUserPwd } from '@/api/sysapi'
    import storage from '@/utils/storage'
    import {mapState} from 'vuex'
    import tips from '@/mock/tips';
    export default {
        components:{
            [NavBar.name]:NavBar,
            [Toast.name]:Toast,
        },
        computed:{
            ...mapState(['language','loginflag']),
        },
        data(){
            return {
                seepw:{
                    pw1:true,
                    pw2:true,
                    pw3:true,
                },
                pwd:{
                    oldPwd:'',
                    newPwd:'',
                },
                newPwd1:'',
                // oldPwdShow:true,
            }
        },
        created() {
            // this.getUserInfoFn()
        },
        methods:{
            onClickLeft(){
                history.go(-1)
            },
            // getUserInfoFn(){ 
            //     getUserInfo().then(res=>{
            //         if(!res.data.password){
            //             this.oldPwdShow = false; //如果是手机短信登录的话没有旧密码
            //         }
            //     })
            // },
            comfireFn(){

                var passwordflag = /^[\S\s]{6,40}$/g.test(this.pwd.oldPwd);
                if(!passwordflag&&this.oldPwdShow){
                    // Toast('旧密码长度最少6位，最长40位')
                    Toast(tips[this.language].oldpwlength)
                    return
                }

                var newPwdpasswordflag = /^[\S\s]{6,40}$/g.test(this.pwd.newPwd);
                if(!newPwdpasswordflag){
                    // Toast('新密码长度最少6位，最长40位')
                    Toast(tips[this.language].newpwlength)
                    return
                }

                if(this.pwd.newPwd!=this.newPwd1){
                    // Toast('两次密码不一样')
                    Toast(tips[this.language].pwDiffernt)
                    return
                }

                updateSysUserPwd(this.pwd).then(res=>{
                    Toast(res.data.message);
                    if(res.data.code == '1'){
                        storage.clear();
                        this.$router.push('/login')
                    }
                })
                
            }
        }
    }
</script>

<style lang="less" scoped>
    #forgetpw{
        padding-top: 46px;
    }
    .zzm-img{
        display: flex; justify-content: space-between; box-sizing: border-box; padding:0 10px 10px 15px;
        img{
            width: 45%; height: .7rem;
        }
        input{
            width: 45%; border: 1px solid #ccc; outline: none; text-indent: 5px;
        }
    }
    .ipt-pic{
        box-sizing: border-box; padding: 0 .2rem; background: #fff; height: .9rem; margin-top: .1rem;
        display: flex; align-items: center; position: relative;
        img{
            width: .36rem;
        }
        input{
            border: none; height: .9rem; width: 5.1rem; text-indent: .2rem; font-size: .24rem;
            color: #666;
        }
        input::-webkit-input-placeholder{
            color: #666;
        }
        button{
            position: absolute; right: .2rem; top: .2rem; height: .5rem; width: 1.4rem; background: #3291ef;
            color: #fff; font-size: .2rem; border: none; outline: none;border-radius: 3px;
        }
        .see{
             position: absolute; right: .2rem; top: .3rem; 
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