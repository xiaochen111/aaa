<template>
    <div id="check">
        <div>
            <div>台式机</div>
            <iframe src="http://eshipping.oss-cn-shanghai.aliyuncs.com/eshippingtest/api/mbl/f3c8ef48963d4b28b3499b435cefeae7.xls" frameborder="0" width="100%"
            
             ></iframe>

            <div>
                <label>邮箱</label>
                <input type="text" v-checkParam="{dataType:'require|email',msg:'不能为空|邮箱不正确'}">
            </div>
            <div>
                <label>邮箱1</label>
                <input type="text" v-checkParam="{dataType:'require',msg:'不能为空'}" >
            </div>
            <!-- id  检验的区域 -->
            <button v-submit="{id:'check'}">提交</button>

            <!-- picker-demo -->
            <div @click="chioseDef('start',0)">
                <input type="text" v-model="start" disabled >
            </div>
            <div @click="chioseDef('end',1)">
                <input type="text" v-model="end" disabled >
            </div>
            <div @click="chioseDef('shpping',2)">
                <input type="text" v-model="shpping" disabled >
            </div>
            <button @click="getItemAll">获取选中项</button>
        </div>

        
        <picker v-if="showPicker"  @item="itemdef" @goback="showPicker = false" :url="url">
        </picker>

        <!-- 
            @item:picker选中项 并 返回结果
            @goback:关闭当前piker界面
            url:请求数据界面
         -->
        
    </div>
</template>

<script>
    import picker from './picker'
    import {portstart,portend,shppingList} from '@/api/dictionary'
    export default {
        components:{
            picker,
        },
        data() {
            return {
                showPicker:false,
                start:'',//起始港
                end:'',//目的港
                shpping:'',//船公司
                selectkey:'', //获取key过度值
                url:'',
                dir:[portstart,portend,shppingList],
                chiose:{} //保存选中的项
            }
        },
        methods: {
            submitdef(){
                console.log('校验正确')
            },
            itemdef(item,itemvalue){ //item 选中项的对象 itemvalue 获取的值
                let key = this.selectkey;
                this.chiose[key] = item; //保存选中的项
                this[key] = itemvalue
                this.showPicker = false;
            },
            chioseDef(key,index){
                this.showPicker = true;
                this.selectkey = key;
                this.url = this.dir[index]
            },
            getItemAll(){
                console.log(this.chiose)
            }
        },
    }
</script>

<style lang="less" scoped>
    input{
        width: 4rem;height: 1rem; border: 1px solid #ccc; outline: none;
    }
</style>