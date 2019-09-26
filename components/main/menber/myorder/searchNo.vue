<template>
    <div id="searchNo">
        <van-search
            v-model="value"
            :placeholder="$t('order.workno')"
            show-action
            @search="onSearch"
            background="linear-gradient(180deg, #20A7FE 0%, #2B92FE 35%, #3482FE 65%, #3E70FE 100%)"
            >
            <div slot="action" @click="onSearch" style="color:#fff">{{$t('common.search')}}</div>
        </van-search>

        <div class="searchNo">
        <div v-if="show">
            <foutdetail :paramsparent="params"></foutdetail>
        </div>
        <!-- <p v-else class="no-data-searchNo">
            暂无数据
        </p> -->
        </div>

    </div>
</template>

<script>
    import { Search,Toast} from 'vant';
    // import detailFcl from './detailComponents'
    import foutdetail from './detailfour.vue';
    import Scroll from '../../../common/scroll'
    import {indexToBookingDetail} from '@/api/getData'
    import {mapState} from 'vuex'
    export default {
        components:{
            [Search.name]:Search,
            [Toast.name]:Toast,
            // detailFcl,
            foutdetail,
            Scroll
        },
        data(){
            return {
                value:'',
                // dataObj:{},
                params:{},
                show:false
            }
        },
        created() {
            
        },
        methods:{
            onSearch(){
                this.initData(this.value)
            },
            initData(value){
                let params = {serialNo:value}
                indexToBookingDetail(params).then(res=>{
                    let flag = Object.keys(res.data).length
                    if(!flag){
                        let tips = this.language == 'En'?'Please re-enter the query data':'请重新输入查询数据'
                        Toast(tips)
                        this.show = false;
                        return
                    }
                    // this.dataObj = res.data[0];
                    this.params = {id:res.data.booking.id};
                    this.show = true;
                })
            }
            
        },
        computed:{
            ...mapState(['language']),  //这个是后台中英文翻译的关键
        },
    }
</script>

<style scoped>
    #searchNo{
        background: #eee;height:100%; overflow: hidden;
    }
    .searchNo{
         height:calc(100% - 50px); overflow: hidden;
    }
    .van-search--show-action{
        margin-bottom: 8px;
    }
    .no-data-searchNo{
        font-size: .22rem; text-align: center; line-height: 30px;
    }
</style>