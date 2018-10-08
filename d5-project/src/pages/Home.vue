<template>
   <div class="wrapper">
       <div class="layout">
        <div class="text">忘记密码</div>
        <button class="buttons">
            <img class="set-img" src="../assets/styles/icons/email.svg">
            <div class="text-desc">邮箱找回</div>
        </button>
        <button class="buttons">
            <img class="set-img" src="../assets/styles/icons/mobile.svg">
            <div class="text-desc">手机号找回</div>
        </button>
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
        position: relative
        .layout
            position: absolute
            left: 50%
            margin-left: -175px
            margin-top: 45px
            .text
                font-family: "SourceHanSansCN-Normal", sans-serif
                font-size: 30px
                color: #5A5E66
                letter-spacing: 0
                line-height: 45px
                height: 30px
                margin-bottom: 40px
            .buttons
                display: inline-block
                background: #fff
                border: 1px solid #d8dce5
                border-radius: 8px
                width: 160px
                height: 160px
                margin-right: 30px
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
</style>


