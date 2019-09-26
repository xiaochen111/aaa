<template>
    <div class="generaldetail">
        <nav-bar :title="addflag?$t('common.add'):$t('common.alter')"></nav-bar>
        <scroll class="recommend-content">
            <div>
                <div class="content-form">
                    <p class="label"> <span class="red">*</span>{{$t('common.Name')}}：</p>
                    <div class="text-area border-bottom">
                        <textarea name="" id="" cols="30" v-model="consignee.name" rows="10" v-checkParam="{dataType:'require',msg:'名称不能为空'}"></textarea>
                    </div>
                    <p class="label"> <span class="red">*</span>{{$t('common.address')}}：</p>
                    <div class="text-area border-bottom">
                        <textarea name="" id="" cols="30" v-model="consignee.address" rows="10" v-checkParam="{dataType:'require',msg:'地址不能为空'}"></textarea>
                    </div>
                    <div class="item-filed border-bottom">
                        <p class="label">{{$t('common.Contact')}}：</p>
                        <div class="ipt">
                            <input type="text" v-model="consignee.contact">   
                        </div>
                    </div>
                    <div class="item-filed border-bottom" @click="choisDef('countryIdCode')">
                        <p class="label">{{$t('common.countries')}}：</p>
                        <div class="ipt">
                            <input type="text" v-model="consignee.countryIdCode" disabled='disabled'>
                        </div>
                    </div>
                    <div class="item-filed border-bottom">
                        <p class="label">{{$t('common.tel')}}：</p>
                        <div class="ipt">
                            <input type="text" v-model="consignee.tel" v-checkParam="{dataType:'phone',msg:'电话格式不正确'}">
                        </div>
                    </div>
                    <div class="item-filed border-bottom">
                        <p class="label">{{$t('common.email')}}：</p>
                        <div class="ipt">
                            <input type="text" v-model="consignee.eMail" v-checkParam="{dataType:'email',msg:'邮箱不正确'}">
                        </div>
                    </div>
                    <div class="item-filed border-bottom">
                        <p class="label">{{$t('common.postalcode')}}：</p>
                        <div class="ipt">
                            <input type="text" v-model="consignee.postalCode">
                        </div>
                    </div>
                </div>

                <div class="item-list">
                    <span>{{$t('common.setdefault')}}</span>
                    <van-switch v-model="checked"/>
                </div>

                <!-- <div class="item-list">
                    <span class="font-col">删除此条发货人信息</span>
                </div> -->

                <div class="btn">
                    <button v-submit>{{$t('common.commit')}}</button>
                </div>
            </div>
        </scroll>

        <picker v-if="showPicker"  @item="itempick" @goback="showPicker = false" :url="countryList">
        </picker>
    </div>
</template>

<script>
    import navBar from '@/components/common/navBar'
    import Scroll from "@/components/common/scroll"
    import picker from "@/components/common/picker"
    import { countryList } from '@/api/dictionary'
    import { iptshade } from '@/utils/common'
    import { Switch } from 'vant'
    import {consignee} from '@/api/getData'
    import Vue from 'vue';
    export default {
        components:{
           navBar,Scroll,
           [Switch.name]:Switch,
           picker,
        },
        data() {
            return {
                showPicker:false,
                consignee:{
                    isDefault:'',
                },
                chiose:{}, //保存选中的项
                countryList,
                selectkey:'',
                addflag:true,//是否为新增
            }
        },
        created() {
            let id = this.$route.query.id;
            if(id){
                this.eidtItem({id});
                this.addflag = false;
            }
        },
        computed: {
            checked:{
                get:function(){
                    return this.consignee.isDefault==1?true:false
                },
                set:function(newvalue){
                    this.consignee.isDefault = newvalue
                }
            }
        },
        methods: {
            submitdef(){ // 新增or修改
                let id = this.$route.query.id;
                if(id){this.consignee.id = id}
                this.consignee.isDefault = this.checked ? '1' : '2';
                if(this.chiose.countryIdCode){this.consignee.countryId = this.chiose.countryIdCode.id;}
                consignee('saveOrUpdate',this.consignee).then(res=>{
                    if(res.data.code=='1'){
                        history.go(-1);
                    }
                })
            },
            // 修改
            eidtItem(params){
                consignee('selectById',params).then(res=>{
                    // res.data.isDefault = res.data.isDefault == 1 ? true : false
                    this.consignee = res.data;
                })
            },
            itempick(item,itemvalue){ //item 选中项的对象 itemvalue 获取的值
                let key = this.selectkey;
                this.chiose[key] = item; //保存选中的项
                this.consignee[key] = itemvalue
                this.showPicker = false;
            },
            choisDef(key){
                this.showPicker = true;
                this.selectkey = key
            },
        },  
        mounted() {
            iptshade();
        },
    }
</script>

<style lang="less" scoped>
    .generaldetail{
        height: 100%; box-sizing: border-box; padding-top: 46px; overflow: hidden;
    }
    .content-form{
        box-sizing: border-box; padding: .1rem .2rem; background: #fff;
        .label{
            font-size: .28rem; color: #666; line-height: .55rem;white-space:nowrap;
            .red{
                color: #fe0215;
            }
        }
        .text-area{
            padding-bottom: .15rem; font-size: 0;
            textarea{
                height: 1.4rem; font-size: .24rem; color: #666; box-sizing: border-box; padding: .1rem; resize: none; width: 100%;
                border:1px solid #cdcccc;
            }
        }
        .item-filed{
            box-sizing: border-box; padding: .1rem 0; display: flex; 
        }
        .ipt{
            width: 100%; font-size: 0;
            input{
                width: 100%;font-size: .28rem; color: #666; line-height: .55rem; border:none; outline: none;
            }
            input[disabled]{
                background: transparent;color: #666;
            }
        }
    }
    .item-list{
        box-sizing: border-box; padding: .1rem .2rem; background: #fff; display: flex; align-items: center; justify-content: space-between;
        font-size: .26rem; color: #666; margin: .15rem 0; line-height: 30px;
        .font-col{
            color: #fe0215;
        }
    }
    .btn{
        padding: .2rem;
    }
    button{
        border: none; outline: none; background: #368ef0; color: #fff; font-size: .28rem; width: 100%;
        display: block;line-height: .8rem; 
    }
</style>