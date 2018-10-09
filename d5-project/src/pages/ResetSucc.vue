<template>
    <div class="wrapper">
        <div>
            <div class="title">密码已重置</div>
            <p class="desc">请牢记新的密码，<br>打开Fusion输入账号及新的密码登录。</p>
            <button class="btn-close" @click="closeWin">关闭</button>
        </div>
    </div>
</template>

<script>
export default {
    name: 'ResetSucc',
    methods: {
        closeWin() {
            this.$router.push('/');
        }
    }
}
</script>

<style lang="stylus" scoped>
    .wrapper
        display: flex
        justify-content: center
        font-family: "SourceHanSansCN-Normal", sans-serif
        letter-spacing: 0
    .title
        font-size: 30px
        color: #5a5e66
        margin-top: 77px
        line-height: 45px
    .desc
        font-size: 14px
        color: #5a5e66
        line-height: 48px
        margin-top: 28px
    .btn-close
        background: #409eff
        border-radius: 4px
        color: #fff
        width: 98px
        height: 36px
        border: none
        font-size: 14px
        margin-top: 22px
</style>

