<template>
    <div class="body">
        <div class="login-app">
            <input type="radio" name="login-opt" id="username" disabled />
            <label for="username" class="u-btn">后台管理 | 校园爱心电商 </label>
            <div class="form-bar">
                <form action="" class="u-form">
                    <input
                        type="text"
                        placeholder="请输入账号名"
                        v-model="user.name"
                    />
                    <input
                        type="password"
                        placeholder="登录密码"
                        v-model="user.password"
                    />
                    <button class="login-btn" @click.prevent="onSubmit">
                        登录
                    </button>
                </form>
            </div>
        </div>
    </div>
</template>

<script lang="ts" setup>
import { signIn } from "@/api/auth"
import { useMessage } from "naive-ui"
import { reactive } from "vue"
import { useRouter } from "vue-router"
import { useStore } from "../store"
import cookies from "../utils/cookies"

const store = useStore()
const router = useRouter()
const message = useMessage()
const user = reactive({
    name: "",
    password: "",
})

async function onSubmit() {
    try {
        const req = await signIn(user)
        const resp = req.data

        if (resp.code === 200) {
            cookies.setItem("auth", resp.data)
            store.jwt = resp.data
            router.replace({ name: "info" })
        } else {
            message.error("登录失败,请联系管理员")
        }
    } catch (e) {
        message.error("登录失败,请联系管理员")
    }
}
</script>

<style scoped>
.body {
    background-color: #ecefff;
    user-select: none;

    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 100vh;
}

.login-app {
    width: 348px;
    margin-bottom: 60px;

    overflow: hidden;
    background-color: #fff;
    border-radius: 12px 12px 20px 20px;
    -webkit-border-radius: 12px 12px 20px 20px;
    -moz-border-radius: 12px 12px 20px 20px;
    -ms-border-radius: 12px 12px 20px 20px;
    -o-border-radius: 12px 12px 20px 20px;

    font-size: 0; /* 行内块在html里的回车造成页面里有空格 */
}

input[type="radio"] {
    display: none;
}
label.m-btn,
label.u-btn,
label.q-btn {
    display: inline-block;
    background-color: #f5f5f6;
    font-size: 18px;
    color: rgba(37, 38, 43, 0.36);

    box-sizing: border-box;
    padding: 16px;
    text-align: center;
    /* width: 33.33%; */
    /* 这个33.3%即使是33.33%都并不是三等分就很明显啊,我P1想吐槽的时候为什么在flex布局中就不明显呢 */
    width: 100%;

    cursor: pointer;
}
.m-btn {
    border-top-left-radius: 12px;
}
.q-btn {
    border-top-right-radius: 12px;
}

#message:checked + .m-btn,
#username:checked + .u-btn,
#qrcode:checked + .q-btn {
    background-color: #fff;
    color: #25262b;
    cursor: default;
}

/* 悲伤的故事不悲伤了! */
#message:checked ~ #form-bar {
    transform: translateX(0);
}

#username:checked ~ #form-bar {
    transform: translateX(-348px);
}

#qrcode:checked ~ #form-bar {
    transform: translateX(-692px);
}

#form-bar {
    display: flex;

    transition: transform 0.6s cubic-bezier(0.175, 0.885, 0.32, 1.275);

    -webkit-transition: transform 0.6s cubic-bezier(0.175, 0.885, 0.32, 1.275);
    -moz-transition: transform 0.6s cubic-bezier(0.175, 0.885, 0.32, 1.275);
    -ms-transition: transform 0.6s cubic-bezier(0.175, 0.885, 0.32, 1.275);
    -o-transition: transform 0.6s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

form {
    flex-shrink: 0;
    width: 100%;

    box-sizing: border-box;
    padding: 22px;

    position: relative;
}

input {
    outline: none;
    width: 100%;
    box-sizing: border-box;
    height: 46px;
    margin-bottom: 16px;
    background-color: rgba(39, 39, 41, 0.04);
    border: 1px solid transparent;
    border-radius: 8px;
    -webkit-border-radius: 8px;
    -moz-border-radius: 8px;
    -ms-border-radius: 8px;
    -o-border-radius: 8px;

    font-size: 14px;
    padding: 1px 12px;
    color: #25262b; /* 看不出来变化吧,哎嘿! */
}
input::placeholder {
    color: rgba(37, 38, 43, 0.36);
}
input:focus {
    border: 1px solid rgba(99, 125, 255, 0.48);
    background-color: #fff;
}

.m-form .vercode-btn {
    padding: 4px 12px;
    border-radius: 6px;
    font-size: 14px;
    color: #637dff;
    background-color: transparent;
    border: 1px solid rgba(99, 125, 255, 0.24);

    position: absolute;
    right: 36px;
    margin: 8px 0;

    -webkit-border-radius: 6px;
    -moz-border-radius: 6px;
    -ms-border-radius: 6px;
    -o-border-radius: 6px;
}

.login-btn {
    width: 100%;
    height: 48px;
    border: none;
    border-radius: 10px;
    background: linear-gradient(
        129.12deg,
        /* 沿着这个角度的线渐变 */ #446dff,
        /* 渐变起始颜色 */ rgba(99, 125, 255, 0.75) /* 渐变终点颜色 */
    ); /* 为什么数值这么奇怪? -因为我直接copy官方的 */

    color: #fff;
    font-size: 16px;

    margin-top: 16px;
    margin-bottom: 0;
}

button {
    cursor: pointer;
}

.m-form #sure {
    display: none;
}
.m-form label.tobesure {
    position: absolute;
    border: 2px solid rgba(132, 133, 141, 0.2);
    box-sizing: border-box;
    width: 16px;
    height: 16px;
    border-radius: 50%;
    cursor: pointer;

    left: 22px;
}
#sure:checked + label.tobesure {
    background-color: #637dff;
}
/* 文字位置安排好了, 现在来整个小勾√ */
label.tobesure::after {
    content: "";
    border: 2px solid #fff;
    position: absolute;
    width: 6px;
    height: 3px;
    border-top: none;
    border-right: none;
    transform: translate(-50%, -50%) rotate(-45deg);
    /* 先平移再旋转好(方便的那个"好")定位... */
    /* 这样就形成了一个勾勾 */
    /* 然后定位至圆圈中间 */
    top: 45%;
    left: 50%;
    /* 等于说重新声明了transform, 所以应该放在一起 */

    -webkit-transform: translate(-50%, -50%) rotate(-45deg);
    -moz-transform: translate(-50%, -50%) rotate(-45deg);
    -ms-transform: translate(-50%, -50%) rotate(-45deg);
    -o-transform: translate(-50%, -50%) rotate(-45deg);
    -webkit-transform: translate(-50%, -50%) rotate(-45deg);
}

#sure:not(:checked) ~ .login-btn {
    opacity: 0.5;
    cursor: not-allowed;
    /* 试了:disabled和unchecked和:not(:checked)都不行,就只好让按钮原本半透明,:checked以后不透明 */
    /* 结果也没用 */
    /* 哦是因为元素顺序, 又忘了, 真丢人 */
    /* 这里懒得复现问题了! 所以是这么个呈现形式! 这是当时的注释! */
}

label.tobesure {
    bottom: 45px;
}
label.tobesure p {
    margin: 0;
    margin-left: 20px;
    width: 280px;

    font-size: 12px;
    line-height: 1.5;
    color: rgba(37, 38, 43, 0.36);

    position: relative;
    bottom: 3px;
    /* relative是相对自己原本的位置移动 */
    /* 这里也就是向上移动3px */
    /* 其实只是为了一点强迫症 */
}
label.tobesure p a {
    text-decoration: none;
    color: #637dff;
    padding: 0 5px;
}

.qrcode {
    width: 128px;
    height: 128px;
    box-shadow: 0px 0px 1px 1px rgba(28, 28, 32, 0.05),
        0px 8px 24px rgba(28, 28, 32, 0.12);
    padding: 8px;
    box-sizing: border-box; /* 就应该写*里, 后悔了, 来不及了 */
    border-radius: 10px;

    position: absolute;
    transform: translate(-50%, -50%);
    top: 40%;
    left: 50%;

    -webkit-border-radius: 10px;
    -moz-border-radius: 10px;
    -ms-border-radius: 10px;
    -o-border-radius: 10px;
    -webkit-transform: translate(-50%, -50%);
    -moz-transform: translate(-50%, -50%);
    -ms-transform: translate(-50%, -50%);
    -o-transform: translate(-50%, -50%);
}
.qrcode img {
    width: 100%;
}
</style>
