<template>
    <div id="fcl">
        <scroll class="recommend-content" :data="list">
            <div>

                <div class="fcl-t">
                    <ul class="clearfix">
                        <li class="lab-ipt"  @click="begin('selectValFcl','begin',$t('common.POL'),'/admin/sys/drop/portList','loadFlag:1')">
                            <label for=""> 
                                <svg class="icon" aria-hidden="true">
                                    <use xlink:href="#icon-qishigang"></use>
                                </svg>
                                {{$t('common.POL')}}
                            </label>
                            <input type="text" disabled="disabled"  v-model="selectValFcl.begin['name'+language]" :placeholder="$t('common.required')">
                            <van-icon name="clear" v-show="selectValFcl.begin.nameCn" @click.stop="selectValClear(['selectValFcl','begin'])"/>
                        </li>
                        <li class="lab-ipt" @click="begin('selectValFcl','end',$t('common.POD'),'/admin/sys/drop/portList','loadFlag:2')">
                            <label for=""> 
                                <svg class="icon" aria-hidden="true">
                                    <use xlink:href="#icon-mudegang"></use>
                                </svg>
                                {{$t('common.POD')}}
                            </label>
                            <input type="text" disabled="disabled"  v-model="selectValFcl.end['name'+language]" :placeholder="$t('common.required')">
                            <van-icon name="clear" v-show="selectValFcl.end.nameCn" @click.stop="selectValClear(['selectValFcl','end'])"/>
                        </li>
                        <li class="lab-ipt" @click="begin('selectValFcl','company',$t('common.shipCom'),'/admin/sys/drop/shppingList')">
                            <label for=""> 
                                <svg class="icon" aria-hidden="true">
                                    <use xlink:href="#icon-chuangongsi"></use>
                                </svg>
                                {{$t('common.shipCom')}}
                            </label>
                            <input type="text" disabled="disabled"  v-model="selectValFcl.company['name'+language]">
                            <van-icon name="clear" v-show="selectValFcl.company.nameCn" @click.stop="selectValClear(['selectValFcl','company'])"/>
                        </li>
                        <li class="lab-ipt" @click="begin('selectValFcl','freightLevel','加价等级','/admin/sys/drop/freightLevelList')">
                            <label for=""> 
                                <svg class="icon" aria-hidden="true">
                                    <use xlink:href="#icon-jiajiadengji"></use>
                                </svg>
                                加价等级
                            </label>
                            <input type="text" disabled="disabled" v-model="selectValFcl.freightLevel['name']">
                            <van-icon name="clear" v-show="selectValFcl.freightLevel.name" @click.stop="selectValClear(['selectValFcl','freightLevel'])"/>
                        </li>
                    </ul>
                    <div class="check-fcl">
                        <van-checkbox-group v-model="nonstopStr">
                            <van-checkbox name="1">{{$t('common.Direct')}}</van-checkbox>
                            <van-checkbox name="2">{{$t('common.Trans')}}</van-checkbox>
                        </van-checkbox-group>
                    </div>
                    <button @click="toList">{{$t('common.search')}}</button>
                </div>
                <div class="histroy">
                    <span class="tit">{{$t('common.histroy')}}</span>
                    <span class="all-clear" @click="clearAll">{{$t('common.empty')}}</span>
                </div>

                <div class="histroy-list">
                    <van-cell-swipe :right-width="65" v-for="(item,index) in list" :key="index" >
                        <van-cell-group >
                            <van-cell :title="item['histroyStr'+language]" @click="toList($event,{portStartId:item.portStartId,portEndId:item.portEndId,index:index},(index+1))" />
                        </van-cell-group>
                        <span slot="right" @click="deleteItem(index)">{{$t('common.delete')}}</span>
                    </van-cell-swipe>
                </div>

            </div>
        </scroll>

        <!-- <div class="delete-style" v-if="pickstyle" @click="hidePickerStyleDef">
            <img src="@/asset/image/other/delete.png" alt="" srcset="" class="delete-pic" :style="'top:'+topPosition+'px'">
        </div> -->
    </div>
</template>

<script>
    import {mapState, mapMutations, mapGetters} from 'vuex';
    import { CheckboxGroup, Checkbox, CellSwipe, CellGroup, Cell,Icon ,Toast} from 'vant';
    import Scroll from '@/components/common/scroll';
    import storage from '@/utils/storage'
    export default {
        components:{
            [Checkbox.name]:Checkbox,
            [CheckboxGroup.name]:CheckboxGroup,
            [CellSwipe.name]:CellSwipe,
            [CellGroup.name]:CellGroup,
            [Cell.name]:Cell,
            [Icon.name]:Icon,
            [Toast.name]:Toast,
            Scroll,
        },
        data(){
            return {
                nonstopStr: [],
                list:[],
                topPosition:0,
                pickstyle:false
            }
        },
        methods:{
            ...mapMutations(['navTabNumSet','selectValClear','resetCompareFcl','noKeepAlive']),
            begin(obj,key,name,url,params){
                console.log(params)
                this.$router.push({
                    path:'/select',
                    query:{obj:obj,key:key,name:name,url:url,params:params},//传参数
                });
                this.navTabNumSet(1)
            },
            deleteItem(index){
                this.list.splice(index,1);
                let fclArr = storage.getObject('fclSerHistroy')||[];
                fclArr.splice(index,1);
                storage.setObject('fclSerHistroy',fclArr);
            },
            clearAll(){
                this.list = [];
                storage.setObject('fclSerHistroy',[]);
            },
            toList(e,paramsHis,index){
                if(!index){
                    if(!this.selectValFcl.begin['name'+this.language]){
                        let tips = this.language == 'Cn'?'请选择起始港':'Please Select POL';
                        Toast(tips)
                        return
                    }
                    if(!this.selectValFcl.end['name'+this.language]){
                        let tips = this.language == 'Cn'?'请选择目的港':'Please Select POD';
                        Toast(tips)
                        return
                    }
                }

                let paramsObj = {
                    portStartId:this.selectValFcl.begin.id,
                    portEndId:this.selectValFcl.end.id,
                    shippingRoleStr:this.selectValFcl.company.id,
                    freightLevelId:this.selectValFcl.freightLevel.id,
                    nonstopStr:this.nonstopStr.join(),
                    index:0, //取缓存的下标
                }
                
                let params = paramsHis || paramsObj;

                this.$router.push({
                    path:'/fclList',
                    query:params,
                })

                // 设置浏览记录缓存
                this.setHistry();
            },
            setHistry(){
                let fclSerHistroy = {
                    portStartId:this.selectValFcl.begin.id,
                    portEndId:this.selectValFcl.end.id,
                    histroyStrCn:((this.selectValFcl.begin.nameCn||'空')+'—'+(this.selectValFcl.end.nameCn||'空')),
                    histroyStrEn:((this.selectValFcl.begin.nameEn||'null')+'—'+(this.selectValFcl.end.nameEn||'null')),
                }
                let matcharr = fclSerHistroy.histroyStrCn.match(/空/g)||[];
                let nullsLen = matcharr.length;
                if(nullsLen == 2)return;

                let fclArr = storage.getObject('fclSerHistroy')||[];
                fclArr.forEach((item,index)=>{
                    if(item.histroyStrCn == fclSerHistroy.histroyStrCn){
                        fclArr.splice(index,1)
                    }
                })
               
                fclArr.unshift(fclSerHistroy);
                storage.setObject('fclSerHistroy',fclArr);
            },
            hidePickerStyleDef(){
                this.pickstyle = false;
                let fingerStyle = storage.getObject('fingerStyle')||{};
                fingerStyle.history = true
                storage.setObject('fingerStyle',fingerStyle)
            }
        },
        computed:{
            ...mapState(['selectValFcl','language']),
        },
        created(){
            console.log(this.selectValFcl)
            this.list = storage.getObject('fclSerHistroy')||[];
            this.resetCompareFcl();
            this.noKeepAlive('fcllist');
        },
        mounted() {
            let fingerStyle = storage.getObject('fingerStyle')||{};
            if(this.list.length&&!fingerStyle.history){
                this.topPosition = document.querySelector('.histroy-list').getBoundingClientRect().top - 66;
                this.pickstyle = true;
            }
        },
    }
</script>

<style lang="less" scoped>
    #fcl{
        height: 100%; overflow: hidden;
    }
    .delete-style{
        height: 100%; width: 100%; position: fixed; left: 0; top: 0; background: rgba(0, 0, 0, .5);
    }
    .delete-pic{
        width: 2.5rem; top: 333px; right: 1rem; position: absolute;
    }
    .fcl-t{
        box-sizing: border-box; padding:.3rem .65rem; background: #fff;
    }
    ul{
        li{
            height: .6rem; display: flex;flex-wrap: nowrap; margin-bottom: .3rem; position: relative;
            label{
                display: flex; align-items: center; width: 1.48rem; line-height: .6rem;font-size: .24rem; color: #666; 
                img{
                    height: .4rem; margin-right: .1rem;
                }
                svg{
                    width: .5rem; height: .5rem;
                }
            }
            input{
                width: 3.45rem; display: inline-block; border:1px solid #959595; border-radius: 4px;
                font-size: .24rem; color: #666;  text-indent: 10px; background: #fff; -webkit-text-fill-color: #666;  -webkit-opacity:1; opacity: 1;  
            }
            i{
                font-size: .3rem; right: .2rem;  position: absolute; top: .14rem;color: #666;
            }
        }
    }
    .check-fcl{
        box-sizing: border-box; padding-left: 1.48rem; height: .5rem;font-size: .24rem;  
        .van-checkbox-group{
            display: flex;flex-wrap: nowrap; 
            div{
                margin-right: .3rem;
            }
            .van-checkbox--round{
                border-radius: 0;
            }
        }
    }
    button{
        width: 100%; height: .7rem; font-size: .3rem; outline: none; border: none;   color: #fff; background: #368ef0; display: block; margin-top: .2rem;
        border-radius: 4px;
    }
    .histroy{
        height: .65rem; background: #eee; line-height: .65rem; display: flex; justify-content: space-between; font-size: .24rem; 
        box-sizing: border-box; padding:0 .3rem;
        .tit{
            color: #b9b9b9;
        }
        .all-clear{
            color: #368ef0;
        }
    }
    .histroy-list{
         box-sizing: border-box;
    }
</style>