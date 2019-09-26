<template>
    <div id="generalnav">
        <nav-bar :title="$t('home.generalinfo')"></nav-bar>
        <ul class="nav-box">
            <li>
                <router-link to="/shipper">
                    <img src="@/asset/image/icon/shipper.png" alt="" srcset="">
                </router-link>
                <p>{{$t('common.shouhuoren')}}</p>
            </li>
            <li>
                <router-link to="/consignee">
                    <img src="@/asset/image/icon/consignee.png" alt="" srcset="">
                </router-link>
                <p>{{$t('common.Consignee')}}</p>
            </li>
            <li>
                <router-link to="/notify">
                    <img src="@/asset/image/icon/notify.png" alt="" srcset="">
                </router-link>
                <p>{{$t('common.Notify')}}</p>
            </li>
            
        </ul>
    </div>
</template>

<script>
    import navBar from '@/components/common/navBar'
    export default {
        components:{
            navBar,
        }
    }
</script>

<style lang="less" scoped>
    #generalnav{
        padding-top: 66px;
    }
    .nav-box{
        display: flex; align-items: center; justify-content: space-around; box-sizing: border-box; padding: 0 .2rem;
        li{
            display: flex; flex-direction: column;
        }
        a{
            display: flex; align-items: center;justify-content: space-around; background: #fff; width: 1.6rem; height: 1.6rem; 
            border-radius: 3px; box-sizing: border-box; padding: .15rem 0;
            img{
                width: 60%;
            }
        }
        p{
            font-size: .24rem; color: #666; text-align: center; margin-top: .2rem;
        }
    }
</style>