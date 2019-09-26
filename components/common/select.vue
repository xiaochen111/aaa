<template>
    <div class="select" id="select">
        <van-nav-bar
        :title="this.infor.name + $t('common.search')"
        :left-text="$t('common.goback')"
        left-arrow
        @click-left="onClickLeft"
        />
 台式机
        <!-- <div class="search-box">
            <div class="ipt-box clearfix">
                <van-icon name="search" />
                <input type="text">
            </div>
            <span>取消</span>
        </div> -->
        <form action="/" class="ser-top">
            <van-search
                v-model="value"
                :placeholder="this.infor.name+$t('common.search')"
                show-action
                background="#fff"
                @keyup="onKeyup"
            >
            <div slot="action" @click="onCancel">{{$t('common.cancel')}}</div>
            </van-search>
        </form>
        <div v-if="infor.key=='company'">
            <van-list>
                <van-cell v-for="item in list" :key="item.id" :title="item.code +'——'+ (item.nameCn == undefined?'':item.nameCn)" @click="selectGo(item)"/>
            </van-list>
        </div>
        <div v-if="infor.key=='freightLevel'">
            <van-list>
                <van-cell v-for="item in list" :key="item.id" :title="item.name" @click="selectGo(item)"/>
            </van-list>
        </div>
        <div v-else>
            <van-list>
                <van-cell v-for="item in list" :key="item.id" :title="item.nameEn +'——'+ item.nameCn" @click="selectGo(item)"/>
            </van-list>
        </div>
       

        <!-- <scroll ref="scroll" class="recommend-content">
            <div>
                 fsdfdsf
            </div>
        </scroll> -->

    </div>
</template>

<script>
    import { NavBar,Icon,List,Cell ,Search} from 'vant';
    import Scroll from './scroll';
    import {mapState, mapMutations, mapGetters} from 'vuex';
    import http from '../../utils/axios';
    import qs from 'qs';
    export default {
        components:{
            [NavBar.name]:NavBar,
            [Icon.name]:Icon,
            [List.name]:List,
            [Cell.name]:Cell,
            [Search.name]:Search,
            scroll,
        },
        data() {
            return {
                value:'',
                list:[],
                infor:{},
                paramslink:{},
            };
        },
        created(){
            this.infor = this.$route.query
            if(this.infor.params){
                var arr = this.infor.params.split(':');
                this.paramslink[arr[0]] = arr[1];
            }
            
            this.dataInit(this.infor.url,this.paramslink);
        },
        methods:{
            ...mapMutations(['selectValSet']),

            onClickLeft() {
                history.go(-1);
            },
            selectGo(item){
                var obj = {
                    key:this.infor.obj,
                    itemKey:this.infor.key,
                    newValObj : item
                }
                this.selectValSet(obj)
                history.go(-1);
            },
            dataInit(url,params){
                console.log(params)
                http.post(url,qs.stringify(params)).then(res=>{
                    this.list = res.data;
                    console.log(this.list)
                })
            },
            onCancel(){
                this.value = ''
                this.dataInit(this.infor.url,this.paramslink);
            },
            onKeyup(e){
                let val = e.target.value;
                let target ={}
                Object.assign(target,{name:val},this.paramslink)
                http.post(this.infor.url,qs.stringify(target)).then(res=>{
                    this.list = res.data;
                })
            },
        },
        computed:{
            ...mapState(['selectValFcl']),
        },

    }
</script>

<style lang="less" scoped>
    .select{
        background: #eee;
    }
    .search-box{
        box-sizing: border-box; padding: .15rem .2rem; height: .8rem; line-height: .8rem; display: flex;justify-content:space-between;align-items:center;
        i{
            font-size: .3rem;  color: #ccc; margin-left: 5px;
        }
        .ipt-box{
            height: .5rem;box-sizing: border-box; background: #fff; border:1px solid #ccc; border-radius: 4px; width: 5.35rem; display: flex;align-items:center;
        }
        input{
            width:5rem; height: .44rem; border: none; margin-left: .2rem; font-size: .24rem;
        }
        span{
            font-size: .24rem; color: #368ef0; line-height: .8rem; display: inline-block; 
        }
    }
    .ser-top{
        margin: .1rem 0;
    }
</style>