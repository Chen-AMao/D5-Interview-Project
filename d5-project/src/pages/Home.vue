<template>
   <div class="wrapper">
        <div class="title">忘记密码</div>
        <div class="layout">
            <button class="buttons one">
                <img class="set-img" src="../assets/styles/icons/email.svg">
                <div class="text-desc">邮箱找回</div>
            </button>
            <router-link to="/reset-password">
                <button class="buttons two">
                    <img class="set-img" src="../assets/styles/icons/mobile.svg">
                    <div class="text-desc">手机号找回</div>
                </button>
            </router-link>
        </div>
   </div>
</template>

<script>
export default {
    name: 'Home'
}
</script>

<style lang="stylus" scoped>
    .wrapper
        position: absolute
        left: 50%
        transform: translateX(-50%)
        margin-top: 45px 
        .title
            font-family: "SourceHanSansCN-Normal", sans-serif
            font-size: 30px
            color: #5A5E66
            letter-spacing: 0
            line-height: 45px
            height: 30px
            margin-bottom: 40px
        .layout
            width: 350px
        .buttons
            display: inline-block
            background: #fff
            border: 1px solid #d8dce5
            border-radius: 8px
            width: 160px
            height: 160px
            &:hover
                border-color: #409eff
            .set-img
                width: 48px
                height: 48px
                margin: 32px auto 26px auto
            .text-desc
                font-family: "SourceHanSansCN-Medium", sans-serif
                font-size: 14px
                color: #5a5e66
                letter-spacing: 0
                margin: 0 auto 40px auto
        .one
            float: left 
            margin-right: 30px
        .two
            float: right
</style>


