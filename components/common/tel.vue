<template>
    <div>
        <ul class="fixed-ul" v-if="!sideNav" ref="tel" :style="{right:dx + 'px',top:dy + 'px'}" 
            @touchmove="mousedown($event)"
            @touchend="mouseover($event)"
            >
            <span v-if="loginflag.flag">sdsd
                <li @click="wwlconsultFn1">
                    <a :href="'tel:'+tel['tel'+language]">
                        <img src="../../../static/images/index/lianxiwomen.png" alt="">
                    </a>
                </li>
            </span>
             台式机
            <span v-else>
                <li @click="wwlconsultFn">
                    <a>
                        <img src="../../../static/images/index/lianxiwomen.png" alt="">
                    </a>
                </li>
            </span>
            <!-- 插槽 -->
            <slot></slot>
        </ul>
        <van-popup v-model="telshow" @click-overlay="overlayfn">
            <div id="tel">
                <span v-if="language=='Cn'">
                    <a  class="item-tel" v-for="(item,index) in contactCn" :key="index" :href="'tel:'+item.tel">
                        <div class="main-content">
                            <p class="contacter"> <i class="iconfont icon-dengluming"></i> 姓名：{{item.name}}</p>
                            <p class="port"> <i class="iconfont icon-chuan"></i> 港口：{{item.port}}</p>
                        </div>
                        <img src="../../../static/images/icon/dianhua.png" alt="" srcset="">
                    </a>
                </span>
                <span v-else>
                    <a  class="item-tel" v-for="(item,index) in contactEn" :key="index" :href="'tel:'+item.tel">
                        <div class="main-content">
                            <p class="contacter">Name：{{item.name}}</p>
                            <p class="port">Port：{{item.port}}</p>
                        </div>
                        <img src="../../../static/images/icon/dianhua.png" alt="" srcset="">
                    </a>
                </span>
            </div>
        </van-popup>
    </div>
</template>

<script>
    import { Popup} from "vant";
    import {mapState} from 'vuex'
    import {contactCn,contactEn} from '@/utils/config'
    import {wwlconsult} from '@/api/getData'
    import bus from '@/utils/eventBus'
    export default {
        components: {
            [Popup.name]: Popup,
        },
        computed:{
            ...mapState(['language','tel','loginflag']),
        },
        props:['sideNav'],
        data(){
            return {
                contactCn,
                contactEn,
                telshow:false,
                loginBoolen:false,//是否登录

                dx:0,
                dy:350,
                time:0,//定时器用10s后向边沿滑动
                handler:0, //定时器句柄
                edgeDirection:'',
                
            }
        },
        created() {
            
        },
       
        mounted() {
            bus.$on('telshow',res=>{
                this.telshow = res;
            })
            
            if(!this.sideNav){
                // document.body.addEventListener('touchmove', function (e) {
                //     e.preventDefault() // 阻止默认的处理方式(阻止下拉滑动的效果)
                // }, {passive: false}) // passive 参数不能省略，用来兼容ios和android
                // document.body.addEventListener('touchmove', this.moveForbitDef, {passive: false}) // passive 参数不能省略，用来兼容ios和android
            }
        },
        methods: {
            wwlconsultFn(){
                this.telshow = true;
                wwlconsult();
            },
            wwlconsultFn1(){
                wwlconsult()
            },
            overlayfn(){
                console.log('guanbi')
            },

            moveForbitDef(e){
                e.preventDefault() // 阻止默认的处理方式(阻止下拉滑动的效果)
            },

            mousedown: function (event) {
                document.body.addEventListener('touchmove', this.moveForbitDef, {passive: false}) // passive 参数不能省略，用来兼容ios和android
                let e = event || window.event
                let self = this
                let element = e.target
                this.$refs.tel.setAttribute('class','fixed-ul')
                // element.setAttribute('class','fixed-ul')
                element.classList.remove("transition")                                                                                                                                                                                                                                                                                                                           
                //e.page 是鼠标距离屏幕左侧的宽度 element.offsetLeft ball距离屏幕左侧的宽度
                let mouseOffsetX = e.touches[0].pageX - element.offsetLeft //鼠标距ball左侧的宽度
                // console.log(mouseOffsetX)
                let mouseOffsetY = e.touches[0].pageY - element.offsetTop
                let pageWidth = document.documentElement.clientWidth

                let pageHeight = document.documentElement.clientHeight

                let elementWidth = element.clientWidth
                let elementHeight = element.clientHeight
                if(self.handler){
                     clearInterval(self.handler)
                }
                document.ontouchmove = function(event){
                
                    let e = event || window.event
                    e.stopPropagation();
                    
                    let mouseX = e.touches[0].pageX
                    let mouseY = e.touches[0].pageY
                    let moveX= mouseX - mouseOffsetX
                    let moveY = mouseY - mouseOffsetY
                    
                    let maxX = pageWidth - element.offsetWidth
                    let maxY = pageHeight - element.offsetHeight

                    // console.log(mouseX,mouseY)
                    self.dx = pageWidth - mouseX - elementWidth/2
                    self.dy = mouseY
                    // self.dx = Math.min(maxX, Math.max(0,moveX))
                    // self.dy = Math.min(maxY, Math.max(0,moveY))
                }
                document.ontouchend = function (e) {
                    // return
                    document.body.removeEventListener('touchmove', self.moveForbitDef) // passive 参数不能省略，用来兼容ios和android
                    let intervalTime = self.time
                    document.ontouchmove = null;
                    document.ontouchend = null;
                    self.handler = setInterval(function(){
                        if(intervalTime > 0 ){
                            intervalTime--
                            }else{
                            clearInterval(self.handler)
                            let elementOffsetRight = pageWidth - element.offsetLeft - elementWidth
                            let elementOffsetBottom = pageHeight - element.offsetTop - elementHeight
                            self.slide2edge(element, element.offsetLeft,elementOffsetRight,element.offsetTop,elementOffsetBottom)
                            }
                    },1000)
                }
            },
            slide2edge:function(element,left,right,top,bottom){
                // console.log(element.classList)
                element.classList.add("transition")
                this.$refs.tel.setAttribute('class','fixed-ul transition');
                // console.log(element,left,right,top,bottom)
                // console.log(this.dx)
                this.dx = 0;
            },
            mouseover:function(event){},
        },
        beforeDestroy(){
            document.body.removeEventListener('touchmove', this.moveForbitDef) // passive 参数不能省略，用来兼容ios和android
        }
    }
</script>

<style lang="less" scoped>
    .fixed-ul.transition{
        transition: all 0.3s ease-in-out;
    }
    #tel{
         font-size: .24rem; box-sizing: border-box; padding: .15rem;
        .item-tel{
            display: flex; width: 100%; height: 1.3rem; font-size: .24rem; justify-content: space-between;
            align-items: center; color: #666;
            &:nth-child(2n){
                background: #eeeeee;
            }
        }
        .main-content{
            display: flex; flex-direction: column; 
            p{
                line-height: .5rem; box-sizing: border-box; text-align: left;
            }
            .contacter{
                i{
                    color:#e17f8c;
                }
            }
            .port{
                i{
                    color:#ffab2e;
                }
            }
        }
    }
    .fixed-ul{
        position: fixed; width: 1.16rem; height:1.14rem; top: 50%;right: -.1rem; transform: translateY(-50%);
        li{
            width: 1.16rem; height:1.14rem;
            img{
                width: 1.16rem; height:1.14rem;
            }
        }
    }
    .van-popup{
        width: 80%;
    }
</style>