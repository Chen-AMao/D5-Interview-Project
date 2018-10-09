<template>
    <div class="wrapper">
        <div class=layout>
            <div class="title">重置密码</div>
            <div class="section-one">
                <div class="desc">请先验证手机</div>
                <form method="post">
                    <input class="input" type="text" placeholder="手机号码" v-model="mobile">
                    <div class="input-mix">
                        <input class="mix-inp" type="text" placeholder="验证码" v-model="code">
                        <input type="button" :disabled="disabledSend" @click="getVerifyCode" 
                        :value="btnText" :class="[mixBtn]">
                    </div>
                </form>
            </div>
            <div class="section-two">
                <div class="desc">设置新密码</div>
                <div class="err-text" v-if="show">请检查两次密码输入是否一致</div>
                <form method="post">
                    <input class="input" type="password" placeholder="6-20位字母与数字组合" v-model="pwdOrigin"
                    required>
                    <br>
                    <input type="password" placeholder="请再次输入新的密码" v-model="pwdRepeat" 
                    @blur="check" @input="check" :class="[errorRep]">
                    <br>
                </form>
                <button  :disabled="disabledFinish" @click="submitPost" :class="[finishBtn]"
                >完成</button>
            </div>
            <div class="text">通过邮箱找回密码</div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
// axios 配置
axios.defaults.timeout = 5000;
axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded;charset=UTF-8';
axios.defaults.baseURL = 'http://test.fusion.d5techs.com:8084';

export default {
    name: 'ResetPwd',
    data () {
        return {
            mobile: '',
            code: '',
            disabledSend: false,
            disabledFinish: true,
            btnText: '发送验证码',
            pwdOrigin: '',
            pwdRepeat: '',
            show: false,
            mixBtn: "mix-btn-true",
            errorRep: 'input',
            finishBtn: 'finish-btn-false'
        }
    },
    methods: {
        // 获取验证码
        getVerifyCode() {
            console.log(this.mobile);
            let param = new URLSearchParams();
            param.append('mobile',this.mobile);
            axios.post('http://test.fusion.d5techs.com:8084/user/findPassword/mobile/sendCode',
                        param,{
                      }).then(this.getVerifyCodeSucc)
                        .catch(this.getVerifyCodeFail);
        },
        // 获取成功处理
        getVerifyCodeSucc(response) { 
            console.log("response.data:"+response.data);
            console.log("response.status:"+response.status);
            console.log("response.statusText:"+response.statusText);
            console.log("response.headers"+response.headers);
            console.log("response.config:"+response.config);
            alert("验证码发送成功");

            // 将按钮设置为不可点击状态
            this.disabledSend = true;
            this.mixBtn = 'mix-btn-false';
            // 60秒倒计时
            let time = 60;
            let counter = setInterval(() => {
                if(time <= 0) {
                    this.disabled = false;
                    this.mixBtn = 'mix-btn-true';
                    this.btnText = "发送验证码";
                    clearInterval(counter);
                } else {
                    this.btnText = '重新发送('+ time + ')s';
                    time--;
                }
            },1000);
        },
        // 获取失败处理
        getVerifyCodeFail(error) {
            console.log("error.response.data:"+error.response.data);
            console.log("error.response.status:"+error.response.status);
            console.log("error.response.headers:"+error.response.headers);
            let code = error.response.data.code;
            console.log(code);
            switch(code) {
                case 2008:
                    alert("手机号码格式错误");
                    break;
                case 2003:
                    alert("手机号码没有注册");
                    break;
                case 2009:
                    alert("30秒之内重复提交");
                    break;
                case 2012:
                    alert("发送短信出错");
                    break;
                default:
                    alert(code+":"+error.response.data.msg);
                    break;
            }
        },
        // 输入检查
        check() { 
            if(this.pwdRepeat === '') {
                this.disabledFinish = true;
                this.show = true;
                this.errorRep = 'errorRep';
                this.finishBtn = 'finish-btn-false';
            } else if(this.pwdOrigin !== this.pwdRepeat){
                this.disabledFinish = false;
                this.show = true;
                this.errorRep = 'errorRep';
                this.finishBtn = 'finish-btn-true';
            } else {
                this.disabledFinish = false;
                this.show = false;
                this.errorRep = 'input';
                this.finishBtn = 'finish-btn-true';
            }
        },
        // 点击按钮提交
        submitPost() {
            if(this.pwdOrigin === this.pwdRepeat) {
                this.disabledFinish = false;
                this.show = false;
                this.errorRep = 'input';
                this.finishBtn = 'finish-btn-true';
                // 检验验证码
                if(!((/^[0-9]{6}$/).test(this.code))) {
                    alert("验证码输入错误");
                }
                // 校验密码
                else if(!((/^[0-9A-Za-z]{6,20}$/).test(this.pwdRepeat))) {
                    alert("密码必须包含6-16个字母和数字");
                }
                else {
                    this.finishBtn = 'finish-btn-true';
                    // 提交数据
                    let param = new URLSearchParams();
                    param.append('mobile',this.mobile);
                    param.append('password',this.pwdRepeat);
                    param.append('code',this.code);
                    axios.post('http://test.fusion.d5techs.com:8084/user/findPassword/mobile/resetPassword',
                            param,{
                        }).then(this.submitPostSucc)
                            .catch(this.submitPostFail);
                }
            } else {
                this.disabledFinish = false;
                this.show = true;
                this.errorRep = 'errorRep';
                this.finishBtn = 'finish-btn-true';
            }
        },
        // 提交成功
        submitPostSucc(response) {
            console.log("response.data:"+response.data);
            console.log("response.status:"+response.status);
            console.log("response.statusText:"+response.statusText);
            console.log("response.headers"+response.headers);
            console.log("response.config:"+response.config);
            alert("密码修改成功");
            this.$router.push('/reset-success');
        },
        // 提交失败
        submitPostFail(error) {
            console.log("error.response.data:"+error.response.data);
            console.log("error.response.status:"+error.response.status);
            console.log("error.response.headers:"+error.response.headers);
            let code = error.response.data.code;
            console.log(code);
            switch(code) {
                case 2007:
                    alert("验证码错误");
                    break;
                case 2016:
                    alert("验证码过期");
                    break;
                case 2006:
                    alert("手机号码没有注册");
                    break;
                default:
                    alert(code+":"+error.response.data.msg);
                    break;
            }
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
        margin-top: 22px
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
    .errorRep
        background: #fff
        border: 1px solid #d72a3e
        border-radius: 4px
        margin-top: 8px
        height: 36px
        width: 275px
        font-size: 14px
        padding-left: 12px
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
            .mix-btn-true
                display: inline-block
                float: right
                border-radius: 0 4px 4px 0
                background: #fff
                border: 1px solid #d8dce5
                width: 40%
                height: 36px
                box-sizing: border-box
                color: #b4bccb
            .mix-btn-false
                display: inline-block
                float: right
                border-radius: 0 4px 4px 0
                background: #f5f7fa
                border: 1px solid #d8dce5
                width: 40%
                height: 36px
                box-sizing: border-box
    .section-two
        margin-top: 19px
        .err-text
            font-family: "SourceHanSansCN-Medium", sans-serif
            font-size: 12px
            color: #d72a3e;
            line-height: 17px
        .finish-btn-true
            background: #409eff
            border-radius: 4px
            color: #fff
            width: 98px
            height: 36px
            margin-top: 24px
            font-family: "SourceHanSansCN-Normal", sans-serif
            font-size: 14px
            border: none
        .finish-btn-false
            background: #cacacb
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

