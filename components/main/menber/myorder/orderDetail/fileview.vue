<template>
    <div class="fileview">
        <nav-bar :title="$t('common.fileview')"></nav-bar>
        <div class="parent">
            <div class="scroll-wrapper">
                <iframe :src="fileUrl" frameborder="0"  width="100%"  scrolling="yes" id="iframedom">
                </iframe>
            </div>
        </div>
    </div>
</template>

<script>
    import navBar from '@/components/common/navBar'
    import { Loadicon } from '@/utils/common'
    export default {
        components:{
            navBar,
        },
        data() {
            return {
                fileUrl:'',
                list:[],
                icon:null
            }
        },
        created() {
            let {baseurl,rootPathCn,filePath} = this.$route.params;
            // this.fileUrl = 'http://eshipping.oss-cn-shanghai.aliyuncs.com/eshippingtest/api/debit/8aa6f7377527b59cd481aa3c1107c4e0.pdf'
            this.fileUrl = baseurl+rootPathCn+'/'+filePath
        },
        mounted() {
            const iframe = document.querySelector('#iframedom')
            this.icon = new Loadicon();
            this.icon.createLoad();
            iframe.addEventListener('load',()=>{
                this.icon.removeCreateLoad();
            })
        },
        destroyed() {
            this.icon.removeCreateLoad();
        },
    
    }
</script>

<style lang="less" scoped>
    .fileview{
        box-sizing: border-box; padding-top: 50px; //height: calc(~'100% - 50px'); overflow: hidden;
    }
    .parent{
        width:902px!important;
    }
    .scroll-wrapper{
        -webkit-overflow-scrolling: touch;
        position: absolute; width: 100%; overflow: auto; 
        height: 100vh;
    }
    .scroll-wrapper iframe{
        min-width: 885px!important; height: 300vh;
        // width: 100%; height: 100%;
    }
</style>