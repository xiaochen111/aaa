<template>
    <div class="tipPos">
        <van-notice-bar mode="closeable" v-if="!inapp&&downloadflag">
            <span v-if="language=='Cn'">
                环世物流APP现已上线，为了更好的体验，建议您点击下载使用！
            </span>
            <span v-else>
                Worldwide Logistics APP is now online, in order to better experience, I suggest you click download use!
            </span>
        </van-notice-bar>
        <a v-if="!inweixin&&isAndroid" href="https://eshipping.oss-cn-shanghai.aliyuncs.com/eshipping/app/wwl.apk" download="download" ref="aload" style="display:none;"></a>   
    </div>
</template>

<script>
    import { NoticeBar } from "vant";
    import {mapState, mapMutations} from 'vuex'
    export default {
        components:{
            [NoticeBar.name]: NoticeBar,
        },
        computed:{
            ...mapState(['downloadflag','language']),
        },
        methods: {
            ...mapMutations(['hidedownloadDef']),
        },
        mounted() {
            let domwrap = document.querySelector('.van-notice-bar__wrap');
            if(domwrap){
                domwrap.addEventListener('click',()=>{
                    if(this.inweixin||this.isIOS){
                        let hrefstr = location.origin+location.pathname+'?#/downloadtips'
                        location.href = hrefstr
                    }else{
                        this.$refs.aload.click();
                    }
                })
            }
            let domclose = document.querySelector('.van-icon-close');
            if(domclose){
                domclose.addEventListener('click',()=>{
                    this.hidedownloadDef();
                })
            }
        },
    }
</script>

<style lang="less" scoped>
</style>