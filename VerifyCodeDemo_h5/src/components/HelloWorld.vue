<template>
    <div id="hello" v-cloak>
        <div>
            <input
                id="loginphone"
                type="tel"
                class="login-phone"
                placeholder="请输入手机号"
                maxlength="11"
                v-model="inputphone"
            />
        </div>

        <div class="valicode-con">
            <input
                id="logincode"
                type="tel"
                class="login-valicode"
                placeholder="请输入验证码"
                maxlength="6"
                v-model="inputvalicode"
            />
            <button
                id="getcode"
                type="button"
                v-bind:disabled="!inputNum"
                v-on:click="reqValicode()"
                v-bind:class="{ 'login-uninputphone': !inputNum, 'login-inputphone': inputNum }"
            >{{getvalicode}}</button>
        </div>
    </div>
</template>

<script>
const TIME_COUNT = 60; //验证码倒计时
export default {
    methods: {
        startValicodeTimer: function() {
            if (!this.timer) {
                this.count_Timer = TIME_COUNT;
                this.timer = setInterval(() => {
                    if (this.count_Timer > 0) {
                        this.inputNum = false;
                        this.getvalicode = this.count_Timer + "s后重新获取";
                        this.count_Timer--;
                    } else {
                        this.inputNum = true;
                        this.getvalicode = "获取验证码";
                        clearInterval(this.timer);
                        this.timer = null;
                    }
                }, 1000);
            }
        },
        //获取验证码
        reqValicode: function() {
            var userphone = this.inputphone;

            //根据工信部2019年最新公布的手机号段严谨判断手机号的正则表达式
            var reg = /^(?:(?:\+|00)86)?1(?:(?:3[\d])|(?:4[5-7|9])|(?:5[0-3|5-9])|(?:6[5-7])|(?:7[0-8])|(?:8[\d])|(?:9[1|8|9]))\d{8}$/;

            if (!reg.test(userphone)) {
                console.log("手机号不符合正则表达式");
                this.$layer.open({
                    content: "请输入有效的手机号",
                    skin: "msg",
                    time: 2
                });
                return false;
            }

            // if (
            //     isNaN(userphone) ||
            //     userphone.indexOf("1") != 0 ||
            //     userphone.length != 11
            // ) {
            //     this.$layer.open({
            //         content: "请输入有效的手机号",
            //         skin: "msg",
            //         time: 2
            //     });
            //     return false;
            // }

            this.startValicodeTimer();

            //TODO 调用服务端获取验证码接口
        }
    },
    data: function() {
        return {
            count_Timer: "",
            timer: null,
            //输入框监听
            inputphone: "",
            inputvalicode: "",
            getvalicode: "获取验证码",
            inputNum: false
        };
    },
    watch: {
        inputphone: function() {
            var phone = this.inputphone;
            if (phone.length == 11) {
                this.inputNum = true;
            } else {
                this.inputNum = false;
            }
        }
    }
};
</script>

<style scoped>
.login-phone {
    width: 8rem;
    height: 1.2rem;
    border: 1px solid #ebebeb;
    border-radius: 0.1rem;
    font-size: 0.35rem;
    text-indent: 0.3rem;
    margin-top: 1rem;
}

.valicode-con {
    margin-top: 0.4rem;
}

.login-valicode {
    height: 1.2rem;
    border: 1px solid #ebebeb;
    border-radius: 0.1rem;
    font-size: 0.35rem;
    width: 4.7rem;
    text-indent: 0.3rem;
}

.login-uninputphone {
    display: inline-block;
    height: 1.2rem;
    line-height: 1.2rem;
    margin-left: 0.2rem;
    border: 1px solid #ebebeb;
    background: #ffffff;
    border-radius: 0.1rem;
    font-size: 0.35rem;
    width: 3rem;
    color: #bbbbbb;
}

.login-inputphone {
    display: inline-block;
    height: 1.2rem;
    line-height: 1.2rem;
    margin-left: 0.2rem;
    border: 1px solid #ebebeb;
    background: #ffffff;
    border-radius: 0.1rem;
    font-size: 0.35rem;
    width: 3rem;
    color: #d8aa6c;
}
</style>