<template>
    <div id="messageDetail">
        <nav-bar title="消息详情"></nav-bar>
        <scroll ref="scroll" class="recommend-content" :data="list" >
            <div class="content-box" v-html="list[0]">
                
            </div>
        </scroll>
    </div>
</template>

<script>
    import navBar from '@/components/common/navBar'
    import Scroll from '@/components/common/scroll'
    import { message } from '@/api/getData'
    export default {
        components:{
            navBar,Scroll
        },
        data() {
            return {
                list:[],
            }
        },
        created() {
            let params = this.$route.query;
            this.getContent(params);
        },
        methods: {
            getContent(params){
                message('queryMessageNoticeDetail',params).then(res=>{
                    let contentmain = res.data.content.replace(/target="_blank"/gm,'')
                    this.list.push(contentmain)


                    // // let contentstr = res.data.content.match(/<body>([\S\s]*)<\/body>/)[0];
                    // let reg = /<body>([\S\s]*)<\/body>/igm;
                    // if(reg.test(res.data.content)){
                    //     let contentmain = RegExp.$1;
                    //     contentmain = contentmain.replace(/<div class="box" .*?>/,'<div>').replace(/padding-left: 350px;/gm,"")
                    //     // let paramstr = contentmain.match(/(subBookingNo=\w+)|(id=\w+)/)[0];
                        
                    //     // let hrefstr = paramstr.search(/subBookingNo/)!=-1?`href="#/myorder?${paramstr}">`:`href="#/myorderDetail?${paramstr}">`
                    //     // contentmain = contentmain.replace(/href.*?>/gm,hrefstr)
                    //     contentmain = contentmain.replace(/target="_blank"/gm,'')
                    //     this.list.push(contentmain)
                    // }else{
                    //     this.list.push(res.data.content)
                    // }
                    // // 重新设置图片字体、大小
                    // let _this = this;
                    // setTimeout(function(){
                    //     let arr = Array.from(document.getElementsByTagName('img'));
                    //     let arrP = Array.from(document.getElementsByTagName('p'));
                    //     arr.forEach(item=>{
                    //         item.style.width = "100%"
                    //     })
                    //     arrP.forEach(item=>{
                    //         item.style.fontSize = "0.26rem"
                    //     })
                    //     _this.list.push(1)
                    // },20)
                })
            }
        },
    }
</script>

<style lang="less">
    #messageDetail{
        padding-top: 46px; height: calc(~'100% - 46px'); overflow: hidden; width: 100%; background: #fff;
        .content{
            font-size: .3rem; 
            img{
                width: 100%;
            }
            p{
                margin: 10px 0;
                span{
                    white-space:nowrap;
                }
            }
        }
        .content-box{
            box-sizing: border-box; padding: .2rem;
        }
        .message-main{
            .tit1{
                font-size: .24rem; margin-bottom: .2rem;
            }
            .context{
                text-indent: 2em;font-size: .24rem;margin-bottom: .2rem; line-height: .35rem;
            }
            .name-wei{
                font-size: .24rem; line-height: .4rem;
            }
        }
    }
    
</style>