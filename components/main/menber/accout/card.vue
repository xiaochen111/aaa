<template>
<div id="card">
    <scroll ref="scroll" class="recommend-content">
        <div id="card-inner">
            <div class="card-bg">
                <div class="top">
                    <i class="iconfont icon-zuojiantou" @click="goback"></i>
                    <span>个人信息</span>
                    <i class="iconfont icon-zuojiantou" style="opacity:0"></i>
                </div>
            </div>

            <div class="content">
                <div class="content-one">
                    <div class="avatar-code">
                        <div class="avatar-intro">
                            <img :src="loginflag.infor.headPic?loginflag.infor.headPic:'http://eshipping.oss-cn-shanghai.aliyuncs.com/eshipping/logo/card.jpg'" alt="" srcset="" width="60">
                            <div class="txt-content">
                                <p class="name">{{loginflag.infor.nameCn}}</p>
                                <p class="conpany">{{loginflag.infor.nameEn}}</p>
                            </div>
                        </div>
                        <img :src="`data:image/png;base64,${loginflag.infor.invitcodePic}`" alt="" srcset="" width="80">
                    </div>
                    <p class="code">
                        推广码：{{loginflag.infor.inviteCode}}
                    </p>
                    <p class="infor-item"><i class="iconfont icon-B-gudingdianhuafei"></i> {{loginflag.infor.tel}}</p>
                    <p class="infor-item"><i class="iconfont icon-shouji"></i> {{loginflag.infor.mobile}}</p>
                    <p class="infor-item"><i class="iconfont icon-youxiang1"></i> {{loginflag.infor.email}}</p>
                </div>

                <div class="content-two">
                    <h4><i class="iconfont icon-level-2-copy"></i> 专业介绍</h4>
                    <p>{{loginflag.infor.remark}}</p>
                </div>
            </div>

        </div>
    </scroll>

    <div class="bottom" v-if="!showShare">
        <router-link to="/userinfor">设置信息</router-link>
        <!-- <a @click="showShare = true">转发名片</a> -->
    </div>

    <div class="share" v-if="showShare">
        <div class="share-box">
            <h4>分享至</h4>
            <div class="share-btn">
                <div class="btn" @click="pyq">
                    <img src="@/asset/image/other/quan@1x.png" alt="@/asset/image/other/quan@2x.png 2x,@/asset/image/other/quan@3x.png 3x" srcset="">
                    <span>朋友圈</span>
                </div>
                <div class="btn">
                    <img src="@/asset/image/other/weixin@1x.png" alt="@/asset/image/other/weixin@2x.png 2x,@/asset/image/other/weixin@3x.png 3x" srcset="">
                    <span>好友</span>
                </div>
            </div>
            <div class="cancel" @click="showShare = false">
                取消
            </div>
        </div>
    </div>
</div>
</template>

<script>
    import Scroll from '@/components/common/scroll'
    import {mapState} from 'vuex'
    export default {
        components:{
            Scroll
        },
        computed:{
            ...mapState(['loginflag']),
        },
        created(){
            // console.log(this.loginflag.infor.invitcodePic)
        },
        data() {
            return {
                showShare:false
            }
        },
        methods: {
            goback(){
                this.$router.push({
                    path:'/accout'
                })
            },
        },
        mounted() {
            if(this.inweixin){
                let _this = this
                let link = `${location.origin}${location.pathname}?#/cardshare?userId=${this.loginflag.infor.id}`
                _this.wx.ready(function () {
                    // 分享给朋友
                    _this.wx.onMenuShareAppMessage({
                        title: _this.loginflag.infor.nameCn,
                        desc: _this.loginflag.infor.remark,
                        link: link,
                        imgUrl:'http://eshipping.oss-cn-shanghai.aliyuncs.com/eshipping/logo/card.jpg',
                    });
                    // 分享到朋友圈
                    _this.wx.onMenuShareTimeline({
                        title: _this.loginflag.infor.nameCn,
                        link: link,
                        imgUrl: 'http://eshipping.oss-cn-shanghai.aliyuncs.com/eshipping/logo/card.jpg',
                    });
                })
            }
        },
    }
</script>

<style lang="less" scoped>
    #card{
        height: 100%; overflow: hidden; box-sizing: border-box; padding-bottom: 1rem;
    }
    #card-inner{
        background-image: -webkit-image-set(url("../../../../asset/image/other/background@1x.png") 1x,
        url("../../../../asset/image/other/background@2x.png") 2x,
        url("../../../../asset/image/other/background@3x.png") 3x);
        background-repeat:no-repeat;
        background-size: contain;
    }
    .recommend-content{
        height:100%;
    }
    .iconfont{
        color: #3291ef; font-size: 20px; vertical-align: middle;
    }
    .card-bg{
        margin-bottom: .4rem; padding-top: .4rem;
        .top{
            width: 100%; display: flex; box-sizing: border-box; padding: 0 .2rem; color: #fff;
            justify-content: space-between; 
            i{
                font-size: 22px; color: #fff;
            }
            span{
                font-size: .3rem;
            }
        }
    }
    .content{
        box-sizing: border-box; padding: 0 .4rem; z-index: 9; position: relative;
    }
    .content-one{
        background: #fff; border-radius: .1rem; 
        box-sizing: border-box; padding: .3rem .2rem;
        .avatar-code{
            display: flex; align-items: center; justify-content: space-between;
        }
        .avatar-intro{
            display: flex; align-items: center; width: 100%;
            .txt-content{
                margin-left: .15rem;
            }
            .name{
                color: #333; font-size: .28rem;
            }
            .conpany{
                color: #666; font-size: .24rem;
            }
        }
        .code{
            text-align: right; font-size: .2rem; color: #666;transform: translateY(-50%); 
        }
        .infor-item{
            box-sizing: border-box; padding-left: .1rem; font-size: .26rem; color: #333; line-height: .5rem;
        }
    }
    .content-two{
        background: #fff; border-radius: .1rem; margin-top: .4rem;
        box-sizing: border-box; padding: .3rem .2rem;
        h4{
            font-weight: normal; font-size: .28rem; color: #333;
        }
        p{
            font-size: .24rem; color: #666; line-height: .35rem; margin-top: .26rem;
        }
    }
    .bottom{
        height: .98rem; border-top: 1px solid #cdcccc; background: #fff; width: 100%; position: fixed; left: 0; bottom: 0;
        display: flex;
        a{
            flex: 1; display: flex; justify-content: space-around; align-items: center; font-size: .28rem; color: #666;
            // &:first-child{
            //     border-right: 1px solid #cdcccc;
            // }
            // &:active{
            //     color: #368ef0; border: 1px solid #368ef0;
            // }
        }
    }
    .share{
        height: 100%; width: 100%; position: fixed; top: 0; left: 0; background: rgba(0, 0, 0, .2);
        .share-box{
            position: absolute; width: 100%;  background: #f4f4f4; bottom: 0; left: 0;
        }
        h4{
            font-size: .3rem;font-weight: normal; color: #333; display: flex;justify-content: space-around; box-sizing: border-box;
            padding: .2rem 0;
        }
        .cancel{
            background: #fff;font-size: .26rem; color: #333;
            justify-content: space-around; box-sizing: border-box; display: flex;
            padding: .3rem 0;
        }
        .share-btn{
            box-sizing: border-box; padding: .2rem .4rem; display: flex; font-size: .24rem; color: #666;
        }
        .btn{
            display: flex; flex-direction: column;justify-content: space-around; margin-right: .6rem;
            span{
                padding: .1rem 0; text-align: center;
            }
        }
    }
</style>