<template>
    <div id="additional">
        <van-nav-bar
            :title="$t('common.addcharge')"
            :left-text="$t('common.goback')"
            left-arrow
            fixed
            @click-left="onClickLeft"
        />

        <div class="form-box">
            <p>
                <label>{{$t('common.ChargeName')}}</label>
                <input type="text" :placeholder="tips[language].feename" v-model="common.chargeName">
            </p>
            <p>
                <label>{{$t('common.unit')}}</label>
                <van-radio-group v-model="common.unit">
                    <van-radio name="1">{{$t('common.Ticket')}}</van-radio>
                    <van-radio name="2">{{$t('common.Singlebox')}}</van-radio>
                </van-radio-group>
            </p>
            <p>
                <label>{{$t('common.Currency')}}</label>
                <van-radio-group v-model="common.currency">
                    <van-radio name="1">USD</van-radio>
                    <van-radio name="2">CNY</van-radio>
                </van-radio-group>
            </p>
            <div v-if="common.unit=='1'">
                <p>
                    <label>{{$t('common.Unitprice')}}</label>
                    <input type="number" :placeholder="tips[language].unitprice" v-model="unit1.billPrice">
                </p>
            </div>
            <div v-if="common.unit=='2'">
                <p>
                    <label>20GP</label>
                    <input type="number" :placeholder="tips[language].unitprice" v-model="unit2.price20">
                </p>
                <p>
                    <label>40GP</label>
                    <input type="number" :placeholder="tips[language].unitprice" v-model="unit2.price40">
                </p>
                <p>
                    <label>45HQ</label>
                    <input type="number" :placeholder="tips[language].unitprice" v-model="unit2.price40hq">
                </p>
            </div>

            <button @click="save">保存</button>

        </div>


    </div>
</template>

<script>
    import { NavBar,RadioGroup, Radio,Toast } from 'vant';
    import {mapState,mapMutations, mapGetters} from 'vuex'
    import tips from '@/mock/tips'
    export default {
        components:{
            [NavBar.name]:NavBar,
            [RadioGroup.name]:RadioGroup,
            [Radio.name]:Radio,
            [Toast.name]:Toast,
        },
        data(){
            return {
                common:{
                    unit:'1',
                    currency:'1',
                    chargeName:'',
                },
                unit1:{
                    billPrice:'',
                },
                unit2:{
                    price20:'',
                    price40:'',
                    price40hq:'',
                },
                freightId:'',
                tips:tips
            }
        },
        created() {
            this.setsurchargehttpFlag(1);
        },
        methods:{
            ...mapMutations(['addSurcharge','setsurchargehttpFlag']),
            onClickLeft(){
                history.go(-1)
            },
            save(){
                if(!this.common.chargeName){
                    Toast(tips[this.language].feename)
                    return
                }
                // let target = this.common.unit == '1'? this.unit1 : this.unit2
                let target = {}
                if(this.common.unit == '1'){
                    if(!this.unit1.billPrice){
                        Toast(tips[this.language].unitprice)
                        return
                    }
                    target = this.unit1
                }else{
                    if(!this.unit2.price20&&!this.unit2.price40&&!this.unit2.price40hq){
                        Toast(tips[this.language].unitprice)
                        return
                    }
                    target = this.unit2
                }

                Object.assign(target,this.common)
                this.addSurcharge(target)
                history.go(-1)
            }
        },
        computed:{
            ...mapState(['language']), 
        },
    }
</script>

<style lang="less" scoped>
    #additional{
        height: 100%; background: #fff; box-sizing: border-box; padding-top: 46px;
    }
    .form-box{
        box-sizing: border-box; padding: .3rem .44rem;
        p{
            display: flex; align-items: center; height: .8rem;
            label{
                font-size: .26rem; color: #666; display: flex; align-items: center; width: 1.1rem;
            }
            input{
                width: 4.3rem; height: .55rem; border:1px solid #ccc; border-radius: 3px; font-size: .24rem; 
                text-indent: .1rem; outline: none; text-align: center;
            }
        }
        button{
            width: 5.45rem; height: .7rem; background: #368ef0; color: #fff;
             font-size: .26rem; outline: none; border: none; display: block; border-radius: 3px; margin-top: .3rem;
        }
    }
</style>