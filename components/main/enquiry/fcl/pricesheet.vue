<template>
    <div class="pricesheet">
        <van-nav-bar
            title="报价单明细"
            :left-text="$t('common.goback')"
            left-arrow
            fixed
            @click-left="onClickLeft"
        />

        <div class="table-box">
            <table border="1" class="tb-box">
                <tbody>
                    <tr>
                        <td width="100">起始港</td>
                        <td>{{item.portStartName}}</td>
                    </tr>
                    <tr>
                        <td>目的港</td>
                        <td>{{item.portEndName}}</td>
                    </tr>
                    <tr>
                        <td>中转港</td>
                        <td>{{item.transportName}}</td>
                    </tr>
                    <tr>
                        <td>船公司</td>
                        <td>{{item.shippingName}}</td>
                    </tr>
                    <tr>
                        <td>起运港港区</td>
                        <td>{{item.portStartName}}</td>
                    </tr>
                    <tr>
                        <td>目的港挂靠</td>
                        <td>{{item.portEndName}}</td>
                    </tr>
                    <tr>
                        <td>截关日期</td>
                        <td>{{item.cutoffDay}}</td>
                    </tr>
                    <tr>
                        <td>生效日期</td>
                        <td>{{item.beginDate}}</td>
                    </tr>
                    <tr>
                        <td>失效日期</td>
                        <td>{{item.endDate}}</td>
                    </tr>
                    <tr>
                        <td>班期</td>
                        <td>{{item.schedule}}</td>
                    </tr>
                    <tr>
                        <td>航程</td>
                        <td>{{item.voyage}}</td>
                    </tr>
                    <tr>
                        <td>货量</td>
                        <td>{{item.numberStr}}</td>
                    </tr>
                    <tr>
                        <td>费用明细</td>
                        <td>{{item.surchargeDetail}}</td>
                    </tr>
                    <tr>
                        <td>总计</td>
                        <td><span class="orange">USD&nbsp;&nbsp;&nbsp;&nbsp;{{item.totalCny}}</span> <span class="darkorange">CNY&nbsp;&nbsp;&nbsp;&nbsp;{{item.totalUsd}}</span></td>
                    </tr>
                </tbody>
            </table>
            <p class="save-p">
                <button @click="saveImg">保存图片</button>
            </p>
        </div>
        
        <van-popup v-model="showImg">
            <img :src="imgUrl" alt="" srcset="" width="100%">
            <p class="imgsave">请长按保存图片</p>
        </van-popup>
    </div>
</template>

<script>
    import { NavBar ,Popup} from 'vant';
    import html2canvas from 'html2canvas';
    import {getExpotInfo} from '../../../../api/getData';
    export default {
        components:{
            [NavBar.name]:NavBar,
            [Popup.name]:Popup,
        },
        data(){
            return {
                imgUrl:'',
                showImg:false,
                item:{}
            }
        },
        created() {
            let params = this.$route.query;
            this.getExpotInfoFn(params);

        },
        methods:{
            onClickLeft(){
                history.go(-1)
            },
            saveImg(){
                html2canvas(document.querySelector(".tb-box")).then(canvas => {
                    var dataUrl  = canvas.toDataURL("image/png");
                    this.imgUrl = dataUrl;
                    this.showImg = true;
                });
            },
            getExpotInfoFn(params){
                getExpotInfo(params).then(res=>{
                    this.item = res.data;
                })
            }
        }
    }
</script>

<style lang="less" scoped>
    .pricesheet{
        background: #fff; box-sizing: border-box; padding-top: 46px;
        .table-box{
            box-sizing: border-box; padding: 10px; 
            table{
                table-layout: fixed; width: 100%; border: 1px solid #cdcccc;border-collapse:collapse; 
                td{
                    font-size: .24rem; color:#666;border: 1px solid #cdcccc; box-sizing: border-box; padding-left: 10px;
                }
                tr{
                    line-height: 30px;
                }
            }
            .orange{
                color: #f59268; margin-right: .6rem;
            }
            .darkorange{
                color: #f36363;
            }
        }
        .imgsave{
             font-size: .22rem; color: #333; text-align: center;
        }
        .save-p{
            text-align: center; display:block; font-size: .22rem; margin: 15px 0;
            button{
                width: 1.4rem; height: .5rem; background: #368ef0; color: #fff;  outline: none; border: none; border-radius: 4px;
            }
        }
    }
</style>