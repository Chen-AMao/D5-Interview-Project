<template>
    <div class="wrapper">
        <div class=layout>
            <div class="title">重置密码</div>
            <div class="section-one">
                <div class="desc">请先验证手机</div>
                <form method="post">
                    <input class="input" type="text" placeholder="手机号码" :value="mobile">
                    <div class="input-mix">
                        <input class="mix-inp" type="text" placeholder="验证码">
                        <input class="mix-btn" type="button" :disabled="disabled" @click="getVerifyCode" :value="btnText">
                    </div>
                </form>
            </div>
            <div class="section-two">
                <div class="desc">设置新密码</div>
                <div class="err-text" v-if="show">请检查两次密码输入是否一致</div>
                <form method="post">
                    <input class="input" type="password" placeholder="6-20位字母与数字组合">
                    <br>
                    <input class="input" type="password" placeholder="请再次输入新的密码">
                    <br>
                </form>
                <router-link to="/reset-success">
                    <button class="finish-btn">完成</button>
                </router-link>
            </div>
            <div class="text">通过邮箱找回密码</div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    name: 'ResetPwd',
    data () {
        return {
            mobile: '',
            disabled: false,
            btnText: "发送验证码"
        }
    },
    methods: {
        // 获取验证码
        getVerifyCode() {
            // axios({
            //     method: 'post',
            //     url: 'http://test.fusion.d5techs.com:8084/user/findPassword/mobile/sendCode',
            //     header: {
            //         'Access-Control-Allow-Origin': '*',
            //         'Access-Control-Allow-Methods': 'GET,POST,PATCH,PUT,OPTIONS'
            //     },
            //     data: {
            //         mobile: this.mobile
            //     }
            // }).then(this.getVerifyCodeSucc)
            //   .catch(err=> {
            //       console.log(err);
            //   });
            // let param = new URLSearchParams();
            // param.append('mobile',this.mobile);
            // axios.post('http://test.fusion.d5techs.com:8084/user/findPassword/mobile/sendCode',
            //             param,{
            //             headers: {
            //                 'Content-Type': 'application/x-www-form-urlencoded'
            //             }
            //           }).then(this.getVerifyCodeSucc)
            //             .catch(err=> {
            //                 console.log(err);
            //           });
            axios.post('http://test.fusion.d5techs.com:8084/user/findPassword/mobile/sendCode', {
                mobile: this.mobile
            })
            .then(function (response) {
                console.log(response);
            })
            .catch(function (error) {
                console.log(error);
            });
        },
        // 获取成功处理
        getVerifyCodeSucc(res) {
            console.log(res);
            // 将按钮设置为不可点击状态
            this.disabled = true;
            // 60秒倒计时
            let time = 60;
            let counter = setInterval(() => {
                if(time <= 0) {
                    this.disabled = false;
                    this.btnText = "发送验证码";
                    clearInterval(counter);
                } else {
                    this.btnText = '重新发送('+ time + ')s';
                    time--;
                }
            },1000);
        }
    },
    computed: {
        show () {
            return false;
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
        margin-top: 32px
    .desc
        font-size: 14px
        color: #5a5e66
        line-height: 24px
        margin-bottom: 16px
    .input 
        background: #fff
        border: 1px solid #d8dce5
        border-radius: 4px
        margin-top: 8px
        height: 36px
        width: 275px
        font-size: 14px
        padding-left: 12px
        &:focus
            border-color: #409eff
    .section-one
        margin-top: 19px
        .input-mix
            margin-top: 8px
            width: 289px
            height: 36px
            font-size: 14px
            .mix-inp
                display: inline-block
                float: left 
                background: #fff
                border-radius: 4px 0 0 4px
                border: 1px solid #d8dce5
                width: 60%
                height: 36px
                padding-left: 12px
                box-sizing: border-box
                &:focus
                    border-color: #409eff
            .mix-btn
                display: inline-block
                float: right
                border-radius: 0 4px 4px 0
                background: #fff
                border: 1px solid #d8dce5
                width: 40%
                height: 36px
                box-sizing: border-box
                color: #b4bccb
    .section-two
        margin-top: 19px
        .finish-btn
            background: #409eff
            border-radius: 4px
            color: #fff
            width: 98px
            height: 36px
            margin-top: 24px
            font-family: "SourceHanSansCN-Normal", sans-serif
            font-size: 14px
            border: none
    .text
        font-family: "SourceHanSansCN-Normal", sans-serif
        font-size: 14px
        color: #409eff
        letter-spacing: 0
        margin-top: 26px
    input::-webkit-input-placeholder
        color: #b4bccb
    input::-moz-input-placeholder
        color: #b4bccb
    input::-ms-input-placeholder
        color: #b4bccb
</style>

