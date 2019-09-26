<template>
    <div id="gender">
         <van-nav-bar title="修改性别" left-arrow   @click-left="onClickLeft">
        </van-nav-bar>

        <ul>
            <li class="border-bottom" @click="select = 'nan'">
                <span>男</span>
                <van-icon name="success" class="icon-right" :class="{'block':select == 'nan'}" />
            </li>
            <li class="border-bottom" @click="select = 'nv'">
                <span>女</span>
                <van-icon name="success" class="icon-right" :class="{'block':select == 'nv'}" />
            </li>
            <li class="border-bottom" @click="select = 'bm'">
                <span>保密</span>
                <van-icon name="success" class="icon-right" :class="{'block':select == 'bm'}" />
            </li>
        </ul>
    </div>
</template>

<script>
    import { NavBar,Icon} from 'vant';
    export default {
        components:{
            [NavBar.name]:NavBar,
            [Icon.name]:Icon,
        },
        data(){
            return {
                select:'nv',
            }
        },
        methods:{
            onClickLeft(){
                history.go(-1)
            },
            choise(){

            }
        }
    }
</script>

<style lang="less" scoped>
    ul{
        background: #fff;padding-left: .2rem; box-sizing: border-box;
        li{
            height: .88rem; display: flex; justify-content: space-between; padding-right: .2rem; box-sizing: border-box;
            color: #666; font-size: .26rem; align-items: center;
        }
        .icon-right{
            color: #1296db; display: none
        }
        .block{
            display: block;
        }
    }
</style>