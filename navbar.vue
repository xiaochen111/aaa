<template>
    <div>
        <van-nav-bar :title="title" left-arrow fixed  @click-left="onClickLeft">
            <div slot="right">
                <slot></slot>
            </div>
        </van-nav-bar>
    </div>
</template>

<script>
    import { NavBar} from 'vant';
    export default {
        components:{
            [NavBar.name]:NavBar,
        },
        props:['title','url'],
        // props:{
        //     title:{
        //         type:String,
        //         default:''
        //     },
        //     url:{
        //         type:String,
        //         default:''
        //     }
        // },
        methods:{
            onClickLeft(){
                if(this.url){
                    this.$router.push({path:this.url,})
                }else{
                    if(history.length>1){
                        history.go(-1);
                    }else{
                        this.$router.push({path:'/platform',})
                    }
                }
            },
        }
    }
</script>

<style scoped>

</style>