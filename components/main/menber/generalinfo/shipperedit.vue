<template>
    <div class="generaldetail">
        <nav-bar :title="addflag?$t('common.add'):$t('common.alter')"></nav-bar>
        <scroll class="recommend-content">
            <div>
                <div class="content-form">
                    <p class="label"> <span class="red">*</span>{{$t('common.Name')}}：</p>
                    <div class="text-area border-bottom">
                        <textarea name="" id="" cols="30" v-model="shipper.name" rows="10" v-checkParam="{dataType:'require',msg:'名称不能为空'}"></textarea>
                    </div>
                    <p class="label"> <span class="red">*</span>{{$t('common.address')}}：</p>
                    <div class="text-area border-bottom">
                        <textarea name="" id="" cols="30" v-model="shipper.address" rows="10" v-checkParam="{dataType:'require',msg:'地址不能为空'}"></textarea>
                    </div>
                    <div class="item-filed border-bottom">
                        <p class="label">{{$t('common.Contact')}}：</p>
                        <div class="ipt">
                            <input type="text" v-model="shipper.contact">   
                        </div>
                    </div>
                    <div class="item-filed border-bottom" @click="choisDef('countryIdCode')">
                        <p class="label">{{$t('common.countries')}}：</p>
                        <div class="ipt">
                            <input type="text" v-model="shipper.countryIdCode" disabled='disabled'>
                        </div>
                    </div>
                    <div class="item-filed border-bottom">
                        <p class="label">{{$t('common.tel')}}：</p>
                        <div class="ipt">
                            <input type="text" v-model="shipper.tel" v-checkParam="{dataType:'phone',msg:'电话格式不正确'}">
                        </div>
                    </div>
                    <div class="item-filed border-bottom">
                        <p class="label">{{$t('common.email')}}：</p>
                        <div class="ipt">
                            <input type="text" v-model="shipper.eMail" v-checkParam="{dataType:'email',msg:'邮箱不正确'}">
                        </div>
                    </div>
                    <div class="item-filed border-bottom">
                        <p class="label">{{$t('common.postalcode')}}：</p>
                        <div class="ipt">
                            <input type="text" v-model="shipper.postalCode">
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

        <!-- <van-dialog v-model="show"  show-cancel-button :before-close="beforeClose" :cancelButtonText="$t('common.cancel')" :confirmButtonText="$t('common.confirm')">
           <p>确定离开</p>
        </van-dialog> -->

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
    import {shipper} from '@/api/getData'
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
                shipper:{
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
                    return this.shipper.isDefault==1?true:false
                },
                set:function(newvalue){
                    this.shipper.isDefault = newvalue
                }
            }
        },
        methods: {
            submitdef(){ // 新增or修改
                let id = this.$route.query.id;
                if(id){this.shipper.id = id}
                this.shipper.isDefault = this.checked ? '1' : '2';
                if(this.chiose.countryIdCode){this.shipper.countryId = this.chiose.countryIdCode.id;}
                shipper('saveOrUpdate',this.shipper).then(res=>{
                    if(res.data.code=='1'){
                        history.go(-1);
                    }
                })
            },
            // 修改
            eidtItem(params){
                shipper('selectById',params).then(res=>{
                    this.shipper = res.data;
                })
            },
            itempick(item,itemvalue){ //item 选中项的对象 itemvalue 获取的值
                let key = this.selectkey;
                this.chiose[key] = item; //保存选中的项
                this.shipper[key] = itemvalue
                this.showPicker = false;
            },
            choisDef(key){
                this.showPicker = true;
                this.selectkey = key
            },





        
            confirmback(){
                //清空浏览器历史记录   //当填写错误的时候 点击返回上一页 禁止返回一次 提示是否放弃填写的内容
                // var state = { title: "title", url: "#/shipperedit" }; 
                // window.history.pushState(state, "title", "#/shipperedit"); 

                let _this = this;
                var state = { title: "title", url: "#/shipperedit" }; 
                window.history.pushState(state, "title", "#/shipperedit？"); 
                window.addEventListener("popstate", function(){
                    _this.show = true
                }, false);                

            },
            beforeClose(action, done) {
                action === 'confirm'?history.go(-1):(this.show = false)
            },
        },  
        mounted() {
            iptshade();
            // Array.from(document.querySelectorAll('input')).forEach(item=>{
            //     item.setAttribute('disabled',true)
            // })
            // this.confirmback();
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