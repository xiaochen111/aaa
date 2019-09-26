<template>
    <div>笔记本
        <div :class="down?'mask down':'mask'" @click="onClickLeft"></div>
        <div :class="down?'content down':'content'" :style="'height:'+height">
                <van-nav-bar class="van-nav-bar-temple"
                    :title="title['title'+language]"
                    :left-text="$t('common.cancel')"
                    @click-left="onClickLeft"
                />
                <form action="/" class="ser-top" v-if="showSearch">
                    <van-search
                        v-model="value"
                        show-action
                        @keyup="onInput"
                    >
                    <div slot="action" @click="onCancel">{{$t('common.cancel')}}</div>
                    </van-search>
                </form>
                <scroll class="recommend-content" :data="list">
                    <ul class="item-box">
                        <li class="border-bottom" v-for="(item,index) in list" :key="index"  >
                            <p v-if="itemkey.length==1" @click.stop="onItem(item,(item[itemkey[0]+language]))">
                                {{item[itemkey[0]+language]}}
                            </p>
                            <p v-else @click.stop="onItem(item,item[itemkey[0]])">
                                {{item[itemkey[0]]}}——{{item[itemkey[1]]}}
                            </p>
                        </li>
                    </ul>
                </scroll>
                 台式机
            </div>
    </div>
</template>

<script>
    import { Search,NavBar} from 'vant';
    import http from '@/utils/axios';
    import qs from 'qs';
    import {mapState, mapMutations, mapGetters} from 'vuex';
    import Scroll from "@/components/common/scroll"
    export default {
        inheritAttrs:false,
        components:{
            [Search.name]:Search,
            [NavBar.name]:NavBar,
            Scroll
        },
        data() {
            return {
                value:'',
                list:[],
                itemkey:[],
                title:{},
                down:false
            }
        },
        props:{
            url:Object,
            showSearch:{
                type:Boolean,
                default:true
            },
            height:{
                type:String,
                default:'70%'
            }
        },
         computed: {
            ...mapState(['language']),
            listeners() {
                return {
                    ...this.$listeners,
                    input: this.onInput,
                };
            },
        },
        methods: {
            onInput(event){
                console.log(this.url)
                let {name} = this.url.other
                this.searchDefList({[name]:this.value})
            },
            onItem(item,itemone){
                this.down = true; 
                setTimeout(()=>{
                    this.$emit('item',item,itemone)         
                },200)
            },
            searchDefList(obj={}){
                let {url,params,other,title} = this.url; 
                console.log(params)
                this.title = title;
                let createParams = Object.create(params)
                let paramsObj = {};
                for(let key in createParams){
                    paramsObj[key] = createParams[key]
                }
                this.itemkey = other.itemkey.split(',');
                Object.assign(paramsObj,obj)
                console.log(paramsObj)
                http.post(url,qs.stringify(paramsObj)).then(res=>{
                    this.list = res.data;
                })
            },
            onCancel(){
                this.value = '';
                let {name} = this.url.other
                this.searchDefList({[name]:this.value})
            },
            onClickLeft(){
                this.down = true; 
                setTimeout(()=>{
                    this.$emit('goback')                
                },400)
            },
        },
        created() {
            this.searchDefList()
        },
    }
</script>

<style lang="less" scoped>
.van-hairline--bottom.van-nav-bar.van-nav-bar-temple{
    background: #368ef0;
}
.mask{
    width: 100%; height: 100%; z-index: 999; position: fixed; left: 0; top:0%; background: rgba(0, 0, 0, .5); overflow: hidden;
    &.down{
        animation: slowdown .8s;
    }
}
.content{
    position: absolute;left: 0; bottom: 0%; width: 100%; height: 70%;  overflow: hidden; background: #fff; animation: slowtop .4s; z-index: 999;
    &.down{
        animation: slowdown .4s;
    }
}
.recommend-content{
    height: calc(~"100% - 92px");
}
.pannel{
    width: 100%; height: .9rem; background: #fff;  display: flex; align-items: center; font-size: .24rem;
    align-items: center; justify-content: space-around;
}
.item-box{
    background: #fff;
    li{
        line-height: .8rem; font-size: .22rem; box-sizing: border-box; padding: 0 .2rem; color: #333;
    }
}

@keyframes slowtop {
    0% {
       bottom: -70%;
    }
    100% {
       bottom: 0;
    }
}
@keyframes slowdown {
    0% {
       opacity: .5;
       bottom: 0%;
    }
    100% {
       opacity: 0;
       bottom: -70%;
    }
}
</style>