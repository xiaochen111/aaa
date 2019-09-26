<template>
    <div id="nickname">
        <van-nav-bar :title="$t('setting.changenickname')" left-arrow :right-text="$t('common.confirm')"  @click-left="onClickLeft" @click-right="onClickRight">
        </van-nav-bar>

        <div class="change-name">
            <input type="text" v-model="loginflag.infor.name">
            <van-icon name="clear" @click="clear" />
        </div>
        <p>
            {{$t('setting.expstr')}}
        </p>
    </div>
</template>

<script>
    import { NavBar,Icon,Toast} from 'vant';
    import {updatePersonInfo,getUserInfo} from '@/api/getData'
    import {mapState} from 'vuex'
    export default {
        components:{
            [NavBar.name]:NavBar,
            [Icon.name]:Icon,
            [Toast.name]:Toast,
        },
        computed:{
            ...mapState(['loginflag']),
        },
        methods:{
            onClickLeft(){
                history.go(-1);
            },
            onClickRight(){
                console.log('确定')

                let flag = /^[\S\s]{2,20}$/g.test(this.loginflag.infor.name);
                if(!flag){
                    Toast('不能少于2-20个字符')
                    return
                } 
                let name = {
                    name : this.loginflag.infor.name,
                }
                updatePersonInfo(name).then(res=>{
                    console.log(res.data)
                    history.go(-1);
                })

            },
            clear(){
                this.loginflag.infor.name = '';
            }
        }
    }
</script>

<style lang="less" scoped>
    .change-name{
        height: .9rem; display: flex; justify-content: space-between; padding:0 .2rem; box-sizing: border-box;
        background: #fff; font-size: .26rem; color: #666; align-items: center; 
        input{
            height: .5rem; border: none;outline: none; color: #666;
        }
    }
    p{
        line-height: .5rem; padding:0 .2rem; box-sizing: border-box; font-size: .2rem; color: #b9b9b9;margin-top: .1rem;
    }
</style>