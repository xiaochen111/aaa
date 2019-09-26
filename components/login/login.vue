<template>
    <div id="login">
        <Scroll class="recommend-content">
            <div>
                <div class="welcome-top">
                    <h4>员工登录</h4>
                    <img src="../../../static/images/login/denglu.png" alt="">
                </div>
                <div class="user-login">
                    <div class="form-box">
                        <p><input type="text" :placeholder="$t('logRegis.UserName')" v-model="infor.userName"  id="username"></p>
                        <p><input type="password" :placeholder="$t('logRegis.Password')" v-model="infor.password" id="password"></p>
                        <p class="ipt-p">
                            <van-checkbox v-model="checked">{{$t('logRegis.remblogin')}}</van-checkbox>
                            <a href="/website/wechat/index.html#/login">客户登录</a>
                        </p>
                        <button @click="login"> <van-loading v-if="userloginflag" color="white" size="25px"/>{{$t('logRegis.login')}}</button>
                    </div>
                </div>
            </div>
        </Scroll>

    </div>
</template>

<script>
    import { Checkbox,Toast,Loading } from 'vant';
    import tips from '@/mock/tips';
    import { syslogin,getUserInfo } from '@/api/sysapi';
    import md5 from 'js-md5';
    import { getCookie,iptshade } from '@/utils/common'
    import storage from '@/utils/storage'
    import Vue from 'vue';
    import {mapState,mapMutations} from 'vuex'
    import {projectname} from '@/utils/config'
    import validator from '@/utils/validator'
    import Scroll from "@/components/common/scroll"
    export default {
        components:{
            [Checkbox.name]:Checkbox,
            [Toast.name]:Toast,
            [Loading.name]:Loading,
            Scroll,
        },
        data(){
            return {
                projectname,
                checked:'',
                infor:{
                    userName:'',
                    password:'',
                },
                md5value:'',
                userlogin:true,
                
                show:false,
                msg:'',

                userloginflag:false
               
            }
        },
        computed:{
            ...mapState(['language']),
        },
        created() {
            let userInfo = storage.getObject('userInfo');
            if(userInfo){
                this.md5value = userInfo.password;
                this.infor = userInfo;
                this.checked = true;
            }
        },
        methods:{
            ...mapMutations(['setTel']),
            login(){
                let username = document.querySelector('#username').value;
                let password = document.querySelector('#password').value;
                if(username==''||password==''){
                    Toast(tips[this.language].unempty); //用户名或密码不能为空
                    return
                }
                let mdpw;
                if(!this.md5value){
                    mdpw = md5(password)
                }else{
                    mdpw = this.md5value == password ? this.md5value : md5(password)
                }
                let params = {
                    userName:username,
                    password:mdpw
                }
                this.postLoginDef(params)
            },
            postLoginDef(params){
                if(this.userloginflag) return
                this.userloginflag = true
                syslogin(params).then(res=>{
                    this.userloginflag = false
                    if(res.data.code == '0'){
                        Toast(res.data.message)
                        return
                    }
                    if(res.data.code == '1'){
                        this.loginSuccesDef();
                        if(this.checked){
                            storage.setObject('userInfo',params); // 记住密码
                        }else{
                            storage.remove('userInfo'); // 清空个人记录
                        }
                    }
                })
            },
            loginSuccesDef(){
                getUserInfo().then(res=>{
                    if(res.data.tel){
                        this.setTel(res.data.tel)
                    }
                    if(res.data.permissionsList){
                        var permissions = {};
                        res.data.permissionsList.forEach(item=>{
                            // item = item.replace(/\\/g,'').replace(/\^/g,'').replace(/\$/g,'');
                            item = item.replace(/\\|\^|\$/g,'');
                            permissions[item] = true;
                        })
                        storage.setObject('permissions',permissions)
                    }
                    this.$router.push('platform');
                })
            },
            
        },
        
        
    }
</script>

<style lang="less" scoped>
    #login{
        height: 100%;background-image: linear-gradient(180deg, #1AC0FE 0%, #2898FE 38%, #377DFE 68%, #3E71FE 100%); overflow: hidden;
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
    .get-password{
        color: #fff; font-size: .26rem; position: absolute; right: .1rem; top: .2rem;
    }
    .welcome-top{
        height: 4.2rem;  box-sizing: border-box; padding-top: .7rem; text-align: center; z-index: 0;
        h4{
            font-size: 0.48rem; line-height: 0.6rem; text-align: center; font-weight: normal; color: #fff; 
        }
        img{
            width: 6.18rem; margin-top: -2.2rem; 
        }
    }
    .mocklogin{
        box-sizing: border-box; padding: 0 .45rem 1rem;z-index: 9; position: relative; color: #fff; font-size: .26rem; // text-align: center;
        
    }
    .form-box{
        box-sizing: border-box; padding: .2rem .45rem .24rem;z-index: 9; position: relative;
        p{
            margin-bottom: .3rem;font-size: .26rem; position: relative;
        }
        input{
            width: 100%; border:1px solid #fff; height: .7rem; background: transparent; position: relative;
            font-size: .26rem; color: #fff; text-indent: .17rem; border-radius: 3px;// display: block;
            box-shadow: 0 0 20px 0px rgba(255, 255, 255, 0.35),inset 0 0 14px 0px rgba(255, 255, 255, 0.35);
        }
        input::-webkit-input-placeholder{
            color:#fff;
        }
        .ipt-p{
            display: flex; justify-content: space-between; align-items: center;
            .forget,a{
                color: #fff; font-size: .26rem; 
            }
           
        }
        button{
            width: 100%; border: none; outline: none; height: .7rem; background: #6db6f6;
            color: #fff; font-size: .26rem; border-radius:3px; display: block;margin-bottom: .3rem;
            .van-loading{
                display: inline-block;
            }
        }
        .register{
           color: #fff; font-size: .26rem; 
        }
        .icon-wh{
            display: flex;justify-content: center; font-size: .26rem; 
            img{
                width: .64rem; margin: 0 .1rem;
            }
        }
        .icon-a-wh{
            display: flex;justify-content: space-between; align-items: center; margin-bottom: 0;
            img{
                width: .64rem;
            }
            span{
                color: #fff; font-size: .26rem; display: flex; align-items: center;
            }
        }
    }
</style>

