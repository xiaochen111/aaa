<template>
    <div id="imcheck">
        <div class="kefu">
            <img src="@/asset/image/other/kefu@2x.png" width="45%" alt="" srcset="">
        </div>
        <p class="big-tit">您尚未开通IM权限，请联系管理员</p>
        <p class="small">TEL:</p>
        <p class="txt">+86  021-60324662</p>
        <p class="small">工作时间：</p>
        <p class="txt">10:00a.m—5:00p.m</p>
        <p class="small">E-mail: </p>
        <p class="txt">sha.tonyyu@worldwide-logistics.cn</p>
    </div>
</template>

<script>
    import {  mapMutations } from "vuex";
    export default {
        methods: {
            ...mapMutations(["navTabNumSet"]),
        },
        created() {
            this.navTabNumSet(3);
        },
    }
</script>

<style lang="less" scoped>
    #imcheck{
       .kefu{
           display: flex; align-items: center; justify-content: space-around; box-sizing: border-box; padding: .5rem 0;
       }
    }
    .big-tit{
        color: #676a6c; font-size: .3rem;text-align: center;line-height: .6rem;margin-bottom: .5rem;
    }
    .small{
        color: #949494; font-size: .2rem; text-align: center; line-height: .4rem;
    }
    .txt{
        color: #676a6c; font-size: .28rem;text-align: center;line-height: .4rem;
    }
</style>