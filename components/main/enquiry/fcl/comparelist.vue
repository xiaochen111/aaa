<template>
    <div id="comparelist">
        <nav-bar :title="$t('common.freightcontrast')"></nav-bar>
        <scroll ref="scroll" class="recommend-content" :data="list">
            <div>
                <p class="tit">{{startEnd}}</p>
                <div class="list-item">
                    <van-checkbox-group v-model="checklist"  @change="change">
                        <van-cell-swipe :right-width="65" v-for="(item,index) in list" :key="index">
                            <div class="content border-bottom">
                                <div class="left">
                                    <van-checkbox  :name="item.freightFclId"></van-checkbox> 
                                    <span>{{item.shippingIdCode}}</span>
                                    <span v-if="language=='Cn'">{{item.schedule|weekInitCn}}</span>
                                    <span v-else>{{item.schedule|weekInitEn}}</span>
                                    <span>{{item.transFlag===1?$t('common.Direct'):$t('common.Trans')}}</span>
                                </div>
                                <a @click="toDetail(item.freightFclId)">{{$t('common.viewdetails')}}</a>
                            </div>
                            <span slot="right" class="delete" @click="deleteItem(index)">{{$t('common.delete')}}</span>
                        </van-cell-swipe>
                    </van-checkbox-group>
                </div>
            </div>
        </scroll>

        <div class="bottom-btn">
            <button @click="onClickLeft" :class="list.length==3?'voidClass':''">{{$t('common.addfreight')}}</button>
            <button class="comfirm" @click="compare">{{$t('common.comfireduibi')}}({{checklist.length}}) </button>
        </div>
    </div>
</template>

<script>
    import { NavBar,CellSwipe,Checkbox,CheckboxGroup,Toast } from 'vant';
    import storage from '../../../../utils/storage';
    import {mapState,mapMutations} from 'vuex'
    import Scroll from '../../../common/scroll';
    import {queryBiJiaList} from '../../../../api/getData';
    import tips from '@/mock/tips'
    import navBar from '@/components/common/navBar'
    export default {
        components:{
            [NavBar.name]:NavBar,
            [CellSwipe.name]:CellSwipe,
            [Checkbox.name]:Checkbox,
            [CheckboxGroup.name]:CheckboxGroup,
            [Toast.name]:Toast,
            Scroll,
            navBar
        },
        data(){
            return {
                startEnd:'',
                checklist:[],
                list:[],
            }
        },
        methods:{
            ...mapMutations(['spliceCompareFcl']),
            onClickLeft(){
                if(this.list.length==3) return
                history.go(-1);
            },
            change(val){
                console.log(this.checklist)
            },
            listInit(){
                this.checklist = this.compareFcl;
                let paramsStr = this.compareFcl.join();
                queryBiJiaList({freightIds:paramsStr}).then(res=>{
                    this.list = res.data;
                })
            },
            toDetail(obj){
                this.$router.push({
                    path:'/fcldetail',
                    query:{freightId:obj},//传参数
                })
            },
            deleteItem(index){
                let arrItem = this.compareFcl[index];
                this.spliceCompareFcl(index);
                this.list.splice(index,1);
                let flag = this.checklist.findIndex(item=>item == arrItem);
                if(flag!=-1){
                    this.checklist.splice(flag,1);
                }
            },
            compare(){
                if(this.checklist.length<2){
                    Toast(tips[this.language].compare2)
                    return
                }
                if(this.checklist.length>3){
                    Toast(tips[this.language].more3)
                    return
                }
                this.$router.push({
                    path:'/compare',
                    query:{freightIds:this.checklist.join()},//传参数
                })
            }
        },
        created() {
            let {index,name} = this.$route.query;
            this.startEnd = index?storage.getObject('fclSerHistroy')[index]['histroyStr'+this.language]:name;
            this.listInit();
        },
        computed:{
            ...mapState(['language','compareFcl']), 
        },

    }
</script>

<style lang="less" scoped>
    #comparelist{
        box-sizing: border-box; padding-top: 46px; height: 100%; overflow: hidden; color: #666;
        .tit{
            height: .6rem; line-height: .6rem; text-align: center; font-size: .26rem; background: #fff;
        }
        .list-item{
            background: #fff; margin-top: .18rem; font-size: .26rem;
        }
        .content{
            height: .8rem; line-height: .8rem; box-sizing: border-box; padding: 0 .1rem; display: flex; justify-content: space-between; align-items: center;
            .van-checkbox{
                display: inline-block; margin-right: .3rem;
            }
            .left{
                display: flex; align-items: center;
                span{
                     display: inline-block; margin-right: .25rem; width: 1.2rem; overflow: hidden;text-overflow: ellipsis; white-space: nowrap; 
                }
            }
            a{
                color: #3291ef;white-space:nowrap;
            }
        }
        .delete{
            height: .8rem; width: 65px; background: #3291ef; color: #fff; text-align: center;  line-height: .8rem; display: inline-block;
        }
        .bottom-btn{
            height: 1.3rem; border-top: 1px solid #cdcccc; position: fixed; bottom: 0; left: 0; width: 100%; background: #fff;
            display: flex; box-sizing: border-box; padding:.35rem .5rem; justify-content: space-between;
            a,button{
                width: 2.3rem; height: .65rem; border: 1px solid #3291ef; border-radius: 5px; outline: none;
                font-size: .26rem; color: #3291ef; background: #fff;
                &.comfirm{
                    color:  #fff; background:#3291ef;text-align: center; line-height: .65rem;font-size: .26rem;  display: inline-block; border: none;
                }
            }
            button.voidClass{
                color: #ccc;border: 1px solid #ccc;
            }
            .comfirm{
                color:  #fff; background:#3291ef;text-align: center; line-height: .65rem;font-size: .26rem;  display: inline-block; border: none;
            }
        }
    }
</style>