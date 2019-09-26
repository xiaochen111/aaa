<template>
    <div id="userinfor">
        <van-nav-bar :title="$t('setting.perinfo')" :left-text="$t('common.goback')" left-arrow fixed  @click-left="onClickLeft">
        </van-nav-bar>
        <ul>
            <li class="border-bottom">
                <div class="avatar">
                    <van-uploader :after-read="onRead">
                        <img v-if="userinfor.headPic" :src="!(new RegExp('null').test(loginflag.infor.headPic))?loginflag.infor.headPic:'./static/images/menber/morentouxiang.png'" alt=""  height="45" width="45">
                        <img v-else :src="!(new RegExp('null').test(loginflag.infor.headPic))?loginflag.infor.headPic:'./static/images/menber/morentouxiang.png'" 
                        alt="" srcset="" height="45" width="45">
                    </van-uploader>
                    <span>点击头像更换</span>
                </div>
                <van-icon name="arrow" class="icon-right" />
            </li>
        </ul>
        <ul>
             <li class="border-bottom">
                <span class="label"> <i class="require">*</i> 中文名</span>
                <input type="text" class="input" v-model="userinfor.nameCn" v-checkParam="{dataType:'require'}" msg="中文名必填">
            </li>
            <li class="border-bottom">
                <span class="label"> <i class="require">*</i> 英文名</span>
                <input type="text" class="input" v-model="userinfor.nameEn" v-checkParam="{dataType:'require'}" msg="英文名必填">
            </li>
            <li class="border-bottom">
                <span class="label"> <i class="require">*</i> 电话</span>
                <input type="text" class="input" v-model="userinfor.tel" v-checkParam="{dataType:'require'}" msg="电话必填">
            </li>
            <li class="border-bottom">
                <span class="label"> <i class="require">*</i> 手机</span>
                <input type="text" class="input" v-model="userinfor.mobile" v-checkParam="{dataType:'require|phone'}" msg="手机必填|手机号码不正确">
            </li>
             <li class="border-bottom">
                <span class="label"> <i class="require">*</i> 邮箱</span>
                <input type="text" class="input" v-model="userinfor.email" v-checkParam="{dataType:'require|email'}" msg="邮箱必填|邮箱不正确">
            </li>
        </ul>
       
        <div class="major-intro">
            <h4><i class="require">*</i> 专业介绍</h4>
            <textarea name="" id="" cols="30" rows="4" v-model="userinfor.remark" v-checkParam="{dataType:'require'}" msg="专业介绍必填"></textarea>
        </div>

        <p class="comfire"><button v-submit>保存</button></p>

        <van-loading color="black" class="pos-load" v-show="load"/>
    </div>
</template>

<script>
    import { NavBar,Icon,Uploader,Toast,Loading } from 'vant';
    import { updateSysUserInfo } from '@/api/sysapi'
    import {upload} from '@/utils/upload'
    import {mapState} from 'vuex'
    export default {
        components:{
            [NavBar.name]:NavBar,
            [Icon.name]:Icon,
            [Uploader.name]:Uploader,
            [Toast.name]:Toast,
            [Loading.name]:Loading,
        },
        data(){
            return {
                load:false,
                userinfor:{}
            }
        },
        computed:{
            ...mapState(['loginflag']),
        },
        created() {
            this.userinfor = this.loginflag.infor
        },
        methods:{
            onClickLeft(){
                history.go(-1)
            },
            onRead(file) {
                let unloadRes = upload(file.file,1, 2,"headpic",Toast)
                this.load = true
                unloadRes.then(res=>{
                    this.userinfor.headPic = res.data.resMap.fileUrl
                    let obj = {
                       headPic:res.data.resMap.fileName,
                    }
                    updateSysUserInfo(obj).then(res=>{
                        this.load = false
                        if(res.data.code=='1'){
                            Toast('上传成功')
                        }
                    }).catch(e=>{
                        this.load = false
                    })
                }).catch(e=>{
                    this.load = false
                })
            },
            submitdef(){
                this.updataUserInfor();
            },
            updataUserInfor(){
                let params = this.userinfor;
                delete params.inviteCode
                delete params.permissionsList
                delete params.headPic
                updateSysUserInfo(params).then(res=>{
                    if(res.data.code == '1'){
                        Toast('修改成功')
                        setTimeout(()=>{
                            this.$router.push({path:'/card'})
                        },1000)
                    }
                })
            }
        }
    }
</script>

<style lang="less" scoped>
    #userinfor{
        padding-top: 46px;
        .major-intro{
            box-sizing: border-box; padding-left: .2rem; background: #fff;font-size: .26rem;padding-right: .2rem;
            h4{
                font-weight: 100; box-sizing: border-box; padding: .2rem 0;color: #666;
            }
            textarea{
                resize: none; width: 100%; border: none; outline: none;color: #333;
            }
        }
        ul{
            box-sizing: border-box; padding-left: .2rem; background: #fff; margin-bottom: .1rem;
            li{
                display: flex;justify-content: space-between; font-size: .26rem; padding: .3rem 0; color: #666;
                align-items: center;box-sizing: border-box; padding-right: .2rem;
            }
            .rig-span{
                color: #cdcccc; display: flex; align-items: center;width: 5rem; justify-content:flex-end;
                i{
                    margin-left: .1rem;
                }
            }
            .label{
                flex-shrink: 0; width: 1.5rem; text-align: left; box-sizing: border-box; padding-right: .4rem; color: #666;
            }
            .input{
                width: 100%; border: none; outline: none; color: #333;
            }
            .avatar{
                display: flex; align-items: center;
                img{
                    margin-right: .2rem;
                }
            }
        }
        .require{
            color:red;
        }
        .pos-load{
            position: fixed; left: 50%; top: 50%; margin-left: -16px; margin-top: -16px;
        }

        .comfire{
            box-sizing: border-box; padding: .3rem;font-size: .34rem;margin-top:.3rem;
            button{
                background: #368ef0; color: #fff; height: .7rem; width: 100%; border: none;
                outline: none; border-radius: 4px;
            }
        }
    }
</style>