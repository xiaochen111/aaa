<template>
    <div id="download">
        <div class="mask" @click="goback" v-if="inweixin">
            <img src="@/asset/image/other/diqiu.png" alt="" srcset="" width="100%">
            <p class="tipxiazai" v-if="language=='Cn'">请点击用手机浏览器打开</p>
            <p class="tipxiazai" v-else>Please click to open it with your mobile browser</p>
            <p class="tipxiazai" v-if="language=='Cn'">下载app应用</p>
            <p class="tipxiazai" v-else>Download app</p>
        </div>
        <div v-else class="download">
            <img src="@/asset/image/other/zuoshangjiao.png" alt="" srcset=""  class="left-top"> 
            <img src="@/asset/image/other/youshangjiao.png" alt="" srcset=""  class="right-top"> 
            <div class="center-main">
                <div v-if="isAndroid">
                    <h4 v-if="language=='Cn'">环世移动端下载</h4>
                    <h4 v-else>Worldwide Logistics mobile download</h4>
                    <img src="@/asset/image/other/Icon.png" alt="" class="icon">
                    <a href="https://eshipping.oss-cn-shanghai.aliyuncs.com/eshipping/app/wwl.apk" download="download" v-if="language=='Cn'">下载环世APP</a>
                    <a href="https://eshipping.oss-cn-shanghai.aliyuncs.com/eshipping/app/wwl.apk" download="download" v-else>Download the APP</a>
                </div>
                <div v-else>
                    <h4 v-if="language=='Cn'">环世移动端下载</h4>
                    <h4 v-else>Worldwide Logistics mobile download</h4>
                    <img src="@/asset/image/other/Icon.png" alt="" class="icon">
                    <a href="https://itunes.apple.com/cn/app/%E7%8E%AF%E4%B8%96%E7%89%A9%E6%B5%81/id1460351465?mt=8"  v-if="language=='Cn'">下载环世APP</a>
                    <a href="https://itunes.apple.com/cn/app/%E7%8E%AF%E4%B8%96%E7%89%A9%E6%B5%81/id1460351465?mt=8"  v-else>Download the APP</a>
                    <!-- <h4 style="margin-bottom:20px" v-if="language=='Cn'">IOS即将上线敬请期待</h4>
                    <h4 style="margin-bottom:20px" v-else>IOS is coming soon</h4>
                    <img src="@/asset/image/other/IOS.png" alt=""  width="40%"> -->
                </div>
            </div>
            <img src="@/asset/image/other/zuoxiajiao.png" alt="" srcset=""  class="left-bottom"> 
        </div>
    </div>
</template>

<script>
    import {mapState} from 'vuex'
    export default {
        computed:{
            ...mapState(['language']),
        },
        methods: {
           goback(){
               history.go(-1)
           }
        },
    }
</script>

<style lang="less" scoped>
    #download{
        height: 100%; width: 100%;
    }
    .tipxiazai{
        font-size: .32rem; color: #fff; line-height: .6rem; text-align: center;
    }
    .mask{
        width: 100%; height: 100%; position: fixed; left: 0; top: 0; background:rgba(0, 0, 0, .9); z-index: 999;
    }
    .download{
        height: 100%; width: 100%; position: relative; background: #f8f8f8;
    }
    .left-top{
        left: 0; top: 0; position: absolute; width: 2rem; top: 1rem;
    }
    .right-top{
        right: 0; top: 0; position: absolute; width: 4rem; 
    }
    .center-main{
        width: 100%; text-align: center; position: absolute; left: 50%; top: 60%; transform: translate(-50%,-50%);
        h4{
            font-weight: normal; font-size: .44rem; color: #368ef0;
        }
        .icon{
            width: 1.6rem;margin-top: 20px;
        }
        a{
            height: 1rem; line-height: 1rem; background: url('~@/asset/image/other/button.png') no-repeat center; background-size: 80%;
            font-size: .34rem; color: #fff; display: block; width: 100%;
        }
    }
    .left-bottom{
        left: 0; bottom: 0; position: absolute; width: 3.2rem;
    }
</style>