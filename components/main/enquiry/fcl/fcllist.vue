<template>
    <div class="list-page" id="list-fcl" ref="wrapper">
       
        <nav-bar :title="title"></nav-bar>
        
        <!-- <tipload></tipload> -->

        <scroll ref="scroll" class="recommend-content" :data="list" :pullup=true @scrollToEnd="onLoad">
            <div class="list-main">
                <div class="dl-s-d" v-for="(item,index) in list" :key="index" @click="toDetail(item.freightFclId)">

                    <div class="item-top">
                        <dl class="clearfix">
                            <dt>
                                <img :src="item.shippingIdOther" alt="" srcset="">
                            </dt>
                            <dd class="dd-1">
                                
                                <span v-if="!item.price20">/</span>
                                <span v-else>{{item.price20 == 999999999?$t('common.call'):item.price20}}</span>

                                <span v-if="!item.price40">/</span>
                                <span v-else>{{item.price40 == 999999999?$t('common.call'):item.price40}}</span>

                                <span v-if="!item.price40hq">/</span>
                                <span v-else>{{item.price40hq == 999999999?$t('common.call'):item.price40hq}}</span>

                            </dd>
                           
                            <dd class="dd-2">
                                <span>20GP</span>
                                <span>40GP</span>
                                <span>40HQ</span>
                            </dd>
                        </dl>
                        <!-- 没登录不给对比运价 -->
                        <!-- <button v-if="loginflag.flag" :class="classBoolean(item.freightFclId)?'compare':''" @click.stop="compare(item.freightFclId,index)">{{classBoolean(item.freightFclId)?$t('common.selected'):$t('common.contrast')}}</button> -->
                    </div>

                    <p class="list-p">
                        <span v-if="language=='Cn'">{{$t('common.Schedule')}}：{{item.schedule|weekInitCn}}</span>
                        <span v-else>{{$t('common.Schedule')}}：{{item.schedule|weekInitEn}}</span>
                        <!-- <span>{{$t('common.port')}}：{{item['wharfIdName'+language]}}</span> -->
                        <span>{{item.portStartIdNameEn}}-{{item.portEndIdNameEn}}</span>
                        
                    </p>
                    <p class="list-p list-compare">
                        <span v-if="item.isValidate == false">{{$t('common.Validate')}}</span>
                        <span v-else style="color:#666">{{$t('common.validity')}}:{{item.beginDate | dateInit('MM-DD-YYYY',language)}}~{{item.endDate | dateInit('MM-DD-YYYY',language)}}</span>
                        <!-- <span style="font-weight:bold;color:#fcc162;">{{item.billType==2?'HBL':'MBL'}}</span>  -->
                        <span>{{$t('common.voyage')}}：{{item.voyage}}{{$t('common.day')}}</span>
                        <span>{{item.routeCode}}</span>
                    </p>
                </div>
                <p class="loading" v-show="loading">{{loadTxt||$t('common.loading')}}</p>
                <p class="loading" v-show="!loading">{{loadTxt||$t('common.loading')}}</p>
            </div>
        </scroll>

        <ul class="filter">
            <li @click="filter">{{$t('common.filtrate')}}</li>
            <li @click="sheetfn('voyage')">{{$t('common.speed')}}</li>
            <li @click="sheetfn('price')">{{$t('common.price')}}</li>
            <li @click="sheetportdef('port')">{{$t('common.port')}}</li>
        </ul>

        <van-actionsheet v-model="sheetAct.show" :title="$t('common.filtrate')">
            <div class="head-set">
                <span @click="reset">{{$t('common.reset')}}</span>
                <span @click="confirm">{{$t('common.confirm')}}</span>
            </div>

            <div class="tab-check border-bottom">
                <van-checkbox-group v-model="nonstopStr">
                    <van-checkbox name="1">{{$t('common.Direct')}}</van-checkbox>
                    <van-checkbox name="2">{{$t('common.Trans')}}</van-checkbox>
                </van-checkbox-group>
            </div>

            <div class="tab-filter">
                <ul class="left-tab">
                    <li :class="{'act':!filterTab}" @click="filterTab=true">{{$t('common.shipCom')}}</li>
                    <li :class="{'act':filterTab}" @click="filterTab=false">{{$t('common.Schedule')}}</li>
                    <!-- <li :class="{'act':filterTab}" @click="filterTab=false">{{$t('common.routes')}}</li> -->
                </ul>
                <div class="choise" v-if="filterTab">
                    <van-checkbox-group v-model="resultBoat">
                        <van-cell-group>
                            <van-cell v-for="item in listBoat" :title="item.code" :key="item.id">
                            <van-checkbox :name="item.id" />
                            </van-cell>
                        </van-cell-group>
                    </van-checkbox-group>
                </div>
                <div class="choise" v-if="!filterTab">
                    <van-checkbox-group v-model="resultDataPick">
                        <van-cell-group>
                            <van-cell v-for="item in DataPick" :title="item['label'+language]" :key="item.id">
                            <van-checkbox :name="item.id" />
                            </van-cell>
                        </van-cell-group>
                    </van-checkbox-group>
                </div>
                <!-- 航线 -->
                <!-- <div class="choise" v-if="!filterTab">
                    <van-checkbox-group v-model="resultLine">
                        <van-cell-group>
                            <van-cell v-for="item in listLine" :title="item['name'+language]" :key="item.id">
                            <van-checkbox :name="item.id" />
                            </van-cell>
                        </van-cell-group>
                    </van-checkbox-group>
                </div> -->
            </div>
        </van-actionsheet>

        <van-actionsheet v-model="sheetAct.voyage" :actions="voyageActions" />
        <van-actionsheet v-model="sheetAct.price" :actions="priceActions" />
        <van-actionsheet v-model="sheetAct.port" :actions="portActions" />

        <!-- <telbox>
             <li @click="linktoCompare" v-if="loginflag.flag">
                <span class="tips" v-if="getCompareFcl.length">{{getCompareFcl.length}}</span>
                <img src="../../../../../static/images/icon/PK.png" alt="">
            </li>
        </telbox> -->
        

        <picker v-if="showPicker"  @item="itemdef" @goback="showPicker = false" :url="url">
        </picker>
        
    </div>
</template>

<script>
    import { Icon,Actionsheet,Checkbox, CheckboxGroup,RadioGroup,List, Radio,Cell,CellGroup,Toast,Popup} from 'vant';
    import Scroll from '@/components/common/scroll';
    import {getQueryFreightFclList,getQueryShippingList,getQueryRouteList,wwlconsult,getUserInfo,queryFreightFclListForFob} from '@/api/getData';
    import { queryFreightFclList,shppingList } from '@/api/sysapi';
    import storage from '@/utils/storage'
    import {mapState,mapMutations, mapGetters} from 'vuex'
    import telbox  from '@/components/common/tel'
    import tipload  from '@/components/common/tipload'
    import picker  from '@/components/common/picker'
    import tips from '@/mock/tips'
    import {permissionsDef} from '@/utils/common'
    import {portstart,portend} from '@/api/dictionary'
    import navBar from '@/components/common/navBar'
    import fcl from '@/components/main/enquiry/fcl/fclsearch';
    export default {
        name:'fcllist',
        components:{
            [Actionsheet.name]:Actionsheet,
            [Checkbox.name]:Checkbox,
            [CheckboxGroup.name]:CheckboxGroup,
            [RadioGroup.name]:RadioGroup,
            [Radio.name]:Radio,
            [Cell.name]:Cell,
            [CellGroup.name]:CellGroup,
            [Icon.name]:Icon,
            [List.name]:List,
            [Toast.name]:Toast,
            [Popup.name]:Popup,
            Scroll,
            telbox,
            tipload,
            picker,
            navBar
        },
        data(){
            return {
                title:'',
                list:[], //运价list
                loading: false,
                loadTxt:'',
                nonstopStr: [], //中转/直航  1.直航 2.中转 
                filterParams:{
                    pageNo:1,
                },
                finished:false, //少于10  不再请求数据
                sheetAct:{ //操作反馈是否显示
                    show:false,
                    voyage:false,
                    price:false,
                    port:false,
                },
                filterTab:true,
                resultBoat:[],
                listBoat:[],
                resultLine:[],
                listLine:[],
                resultDataPick:[],
                DataPick:[{id:1,labelCn:'周一',labelEn:'Mon.'},{id:2,labelCn:'周二',labelEn:'Tue.'},{id:3,labelCn:'周三',labelEn:'Wed.'},{id:4,labelCn:'周四',labelEn:'Thu.'}
                ,{id:5,labelCn:'周五',labelEn:'Fri.'},{id:6,labelCn:'周六',labelEn:'sat.'},{id:7,labelCn:'周日',labelEn:'Sun.'}],
                voyageActions:[{name:'从低到高',callback:this.rankCb,rank:'voyage ASC',sheetAct:'voyage'},{name:'从高到低',callback:this.rankCb,rank:'voyage DESC',sheetAct:'voyage'}],
                priceActions:[{name:'从低到高',callback:this.rankCb,rank:'PRICE_20 ASC',sheetAct:'price'},{name:'从高到低',callback:this.rankCb,rank:'PRICE_20 DESC',sheetAct:'price'}],
                portActions:[{name:'起始港',sheetAct:'port',callback:this.chioseDef,params:{key:'portStartId',index:0}},
                             {name:'目的港',sheetAct:'port',callback:this.chioseDef,params:{key:'portEndId',index:1}}],
                compareBoolean:[],

                showPicker:false,
                start:'',//起始港
                end:'',//目的港
                selectkey:'', //获取key过度值
                url:'',
                dir:[portstart,portend],
                chiose:{}, //保存选中的项
            }
        },
        methods:{
            ...mapMutations(['setCompareFcl','spliceCompareFcl','resetSurcharge','keepAlivedef']),
            onClickLeft() {
                history.go(-1);
            },
            filter(){
                this.sheetAct.show = true;
            },
            sheetfn(str){
                let nameCn = ['从低到高','从高到低']
                let nameEn = ['From low to high','From high to low']
                this[str+'Actions'].forEach((item,index)=>{
                    item.name = eval('name'+this.language)[index]
                })
                this.sheetAct[str] = true;
            },
            // 排序方法
            rankCb(item){
                this.sheetAct[item.sheetAct] = false;
                this.filterParams.orderByClause = item.rank
                this.setSearch();
            },
            sheetportdef(str){
                let nameCn = ['起始港','目的港']
                let nameEn = ['POL','POD']
                this[str+'Actions'].forEach((item,index)=>{
                    item.name = eval('name'+this.language)[index]
                })
                this.sheetAct[str] = true;
            },
            chioseDef(item){
                let {key,index} = item.params
                this.showPicker = true;
                this.selectkey = key;
                this.url = this.dir[index]
                this.sheetAct[item.sheetAct] = false;
            },
            itemdef(item,itemvalue){ //item 选中项的对象 itemvalue 获取的值
                let key = this.selectkey;
                this.chiose[key] = item; //保存选中的项
                this[key] = itemvalue
                this.showPicker = false;
                //初始化运价列表
                this.filterParams[key] = item.id;
                this.setSearch();
                if(this.title.search(/—/)!=-1){
                    if(key=='portStartId'){
                        this.title = this.title.replace(/.*(?=—)/,item['name'+this.language])
                    }else{
                        this.title = this.title.replace(/—.*/,'—'+item['name'+this.language])
                    }
                }
            },
            confirm(){
                // console.log(this.resultBoat)
                this.sheetAct.show = false;

                // 中转、直达
                let nonstopStr = this.nonstopStr.join();

                // 船公司
                let shippingIdsStr = this.resultBoat.join();

                // 航线
                // let routeIdsStr = this.resultLine.join(); {routeIdsStr:routeIdsStr}

                // 班期
                let schedule = this.resultDataPick.join();

                Object.assign(this.filterParams,{nonstopStr:nonstopStr},{shippingRoleStr:shippingIdsStr},{schedule:schedule})

                this.setSearch();
            },
            reset(){
                this.resultBoat = []; //船公司
                this.nonstopStr = []; //中转，直达
                // this.resultLine = []; //航线
                this.resultDataPick = []; //班期
            },
            toDetail(obj){
                let {freightLevelId} = this.$route.query;
                this.$router.push({
                    path:'/fcldetail',
                    query:{freightFclId:obj,freightLevelId},//传参数
                })
            },
            onLoad(){
                this.loading = true;
                this.loadTxt = this.language=="Cn"?'加载中。。':'loading..';
                if(this.finished){
                    this.loading = false;
                    this.loadTxt = this.language=="Cn"? '数据全部加载完成':'The data is fully loaded';
                    return
                }
                this.getQueryFreightFclListFn(this.filterParams)
            },
            // 获取运价数据
            getQueryFreightFclListFn(params={}){
                for(let key in params){
                    if(!params[key]){
                        delete params[key]
                    }
                }
                this.forFclList(params);
            },
            forFclList(params){
                queryFreightFclList(params).then(res=>{
                    this.filterParams.pageNo++;
                    // console.log(res.data);
                    this.list = this.list.concat(res.data);
                    if(res.data.length<10){
                        this.loadTxt = this.language=="Cn"? '数据全部加载完成':'The data is fully loaded';
                        this.finished = true;
                    }
                    if(!res.data.length){
                        this.loadTxt = this.language=="Cn"?'暂无数据。。。':'Temporarily no data';
                    }
                    this.loading = false;
                })
            },
            // 获取船公司
            getQueryShippingFn(){
                shppingList().then(res=>{
                    this.listBoat = res.data;
                })
            },
            // 获取航线
            getQueryRouteListFn(){
                getQueryRouteList().then(res=>{
                    this.listLine = res.data;
                })
            },
            setSearch(){
                this.filterParams.pageNo = 1;
                this.list = [];
                this.finished = false;
                this.getQueryFreightFclListFn(this.filterParams);
            },
            compare(id,index){
                if(this.classBoolean(id)){ //若已选中
                    let i = this.getCompareFcl.findIndex((item)=> item == id);
                    this.spliceCompareFcl(i);
                }else{
                    this.setCompareFcl(id);
                }
                this.compareBoolean[index] = !this.compareBoolean[index];
                console.log(this.getCompareFcl)
            },
            classBoolean(id){
               return this.getCompareFcl.some(item=> item == id)  //根据id来判断之前有么有被选中 
            },
            linktoCompare(){
                if(this.getCompareFcl.length<2){
                    Toast(tips[this.language].compare2)
                    return
                }
                if(this.getCompareFcl.length>3){
                    Toast(tips[this.language].more3)
                    return
                }

                let index,name;
                if(location.hash.search(/index=\d+/g)===-1){
                    name = this.title;
                }else{
                    index = location.hash.match(/index=\d+/g)[0].match(/\d/);
                }
                
                this.$router.push({
                    path:'/comparelist',
                    query:{index,name,},
                })
            },
            
           
        },
        computed:{
            ...mapState(['language','tel','permissions','loginflag']),  //这个是后台中英文翻译的关键
            ...mapGetters(['getCompareFcl']),
        },
        created() {

            let queryparem = this.$route.query;
            if(queryparem.name){
                this.title = queryparem.name;
                delete queryparem.name;
            }else{
                this.title = storage.getObject('fclSerHistroy')[queryparem.index]['histroyStr'+this.language];
                delete queryparem.index;
            }
            //初始化运价列表
            Object.assign(this.filterParams,queryparem)

            this.getQueryFreightFclListFn(this.filterParams);

            // 查询已存在运价的所有船公司
            this.getQueryShippingFn();

            // 查询已存在运价的所有航线
            // this.getQueryRouteListFn()

            // 清空临时附加费
            this.resetSurcharge();

            this.keepAlivedef('fcllist')
        },
    }
</script>

<style lang="less" scoped>
    .van-popup{
        width: 80%;
    }
    .recommend-content{
       height:100%;overflow:hidden;
    }
    .right-top{
        line-height: 46px;
        img{
            width: .3rem; height: .3rem; margin-left: .2rem; vertical-align: middle;
        }
    }
    .top-icon{
        height: 46px; top: 0; width: 100%; position: absolute;
    }
    .list-page{
         background: #eee;
    }

    .item-top{
        display: flex; align-items: center;

        button{
            width: .82rem; height: .4rem; border-radius: 3px; border:1px solid #cdcccc; background: #fff; color: #666;font-size: .18rem;
            &.compare{
                border:1px solid #3291ef; color: #3291ef;
            }
        }
    }
    .dl-s-d{
        box-sizing: border-box; padding: .2rem; background: #fff; margin-top: .1rem;position: relative;
        .jt{
            position: absolute; right: 10px; top: 50%; margin-top: -13px; display: inline-block; font-size: .45rem;color: #666;
        }
    }
    .loading{
        line-height: .44rem; font-size: .18rem; text-align: center;color: #666;
    }

    .list-main{
        box-sizing: border-box; padding-bottom: .88rem; z-index: 0;
        dl{
            width: 100%;
            dt{
                width: .7rem; height: .7rem;float: left; display: flex; justify-content: center; flex-direction:column;
                align-items: center;
                img{
                   width: .7rem; height: .7rem; 
                }
                &.dt{
                    color: #333; font-size: .3rem;  display: flex; justify-content: space-around; align-content: center;
                }
            }
            dd{
                height: .32rem;  font-size: .24rem; box-sizing: border-box;
                padding-left: .1rem; line-height: .32rem; width: calc(~'100% - .75rem'); margin-left: .75rem;
            }
            .dd-1{
                display: flex; justify-content:space-around; line-height: .45rem; height: .45rem;
                span{
                    font-size: .3rem; color: #e53939;
                }
            }
            .dd-2{
                display: flex; justify-content:space-around;
                span{
                    font-size: .2rem; color: #666;
                }
            }
            .dd-3{
                 width: 5rem;
                span{
                   font-size: .18rem; color: #959595; margin-right: .1rem;
                }
            }
        }
        .list-p{
            font-size: .18rem; color: #949494; display: flex; justify-content:space-between; line-height: .4rem; 
            height: .4rem; box-sizing: border-box; padding-right: .4rem;
        }
        .list-compare{
            width: 100%; display: flex; justify-content: space-between; box-sizing: border-box; padding-right: .4rem;
        }
    }
    .filter{
        height: .88rem; position: fixed; bottom: 0; left: 0; width: 100%; background: #666; display: flex;
        justify-content:space-around; z-index: 9;
        li{
            color: #fff; font-size: .24rem; text-align: center; width: 100%/4; line-height: .88rem; position: relative;
            &:after{
                content: ''; display: block; border-right: 1px solid #fff; height: .44rem; right: 0; top: .22rem;
                position: absolute;
            }
            &:last-child{
                &:after{
                    border-right: 1px solid #666;
                }
            }
        }
    }
    .tab-filter{
        display: flex;flex-direction: row; 
        ul.left-tab{
            width: 1.8rem;
            li{
                height: 2.45rem; font-size: .26rem; line-height: 2.45rem; text-align: center;
                &.act{
                    background: #eee;
                }
            }
        }
        .choise{
            width: 4.6rem; height: 4.9rem; overflow: auto;
        }
    }
    .fixed-ul{
        position: fixed; width: 1.16rem; height:1.14*2rem; top: 80%;margin-top: -1.14rem; right: 0;
        li{
            width: 1.16rem; height:1.14rem; position: relative;
            .tips{
                display: inline-block; height: 20px; width: 20px; border-radius: 50%; color: #fff; font-size: .18rem;
                 background: #3291ef; line-height: 20px;text-align: center;position: absolute; right: 5px; top: -5px;
            }
            img{
                width: 1.16rem; height:1.14rem;
            }
        }
    }
</style>