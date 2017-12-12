<template>
  <div class="login-container">
    <el-form class="card-box login-form" autoComplete="on" :model="loginForm" :rules="loginRules" ref="loginForm" label-position="left">
      <h3 class="title">系统登录</h3>
      <div class='tips'>账号： admin 或 editor ; 密码随意</div>
      <el-form-item prop="username">
        <span class="svg-container svg-container_login">
          <svg-icon icon-class="user" />
        </span>
        <el-input name="username" type="text" v-model="loginForm.username" autoComplete="on" placeholder="账号" class="login-input"/>
      </el-form-item>

      <el-form-item prop="password">
        <span class="svg-container">
          <svg-icon icon-class="password" />
        </span>
        <el-input name="password" :type="pwdType" @keyup.enter.native="handleLogin" v-model="loginForm.password" autoComplete="on"
          placeholder="密码" class="login-input" />
        <span class="show-pwd" @click="showPwd"><svg-icon icon-class="eye" /></span>
      </el-form-item>

      <el-button type="primary" class="login-btn"
      :class="{'login-btn-loading':loading==='loading','login-btn-success':loading==='success'}"
      @click.native.prevent="handleLogin">登 录</el-button>
      <a @click='showDialog=true' class="third-part-login"style="text-decoration:underline;">打开第三方登录</a>

      <div class="tips">账号:admin 密码随便填</div>
      <div class="tips">账号:editor  密码随便填</div>

      <el-button class="thirdparty-button" type="primary" @click="showDialog=true">打开第三方登录</el-button>
    </el-form>

    <el-dialog title="第三方验证" :visible.sync="showDialog">
      本地不能模拟，请结合自己业务进行模拟！！！<br/><br/><br/>
      邮箱登录成功,请选择第三方验证<br/>
      <social-sign />
    </el-dialog>
    <div class="wave"></div>
    <div class="wave"></div>
  </div>
</template>

<script>
import { isvalidUsername } from '@/utils/validate'
import socialSign from './socialsignin'

export default {
  components: { socialSign },
  name: 'login',
  data() {
    const validateUsername = (rule, value, callback) => {
      if (!isvalidUsername(value)) {
        callback(new Error('请输入正确的用户名'))
      } else {
        callback()
      }
    }
    const validatePassword = (rule, value, callback) => {
      if (value.length < 6) {
        callback(new Error('密码不能小于6位'))
      } else {
        callback()
      }
    }
    return {
      loginForm: {
        username: 'admin',
        password: '1111111'
      },
      loginRules: {
        username: [
          { required: true, trigger: 'blur', validator: validateUsername }
        ],
        password: [
          { required: true, trigger: 'blur', validator: validatePassword }
        ]
      },
      pwdType: 'password',
      loading: false,
      showDialog: false
    }
  },
  methods: {
    showPwd() {
      if (this.pwdType === 'password') {
        this.pwdType = ''
      } else {
        this.pwdType = 'password'
      }
    },
    handleLogin() {
      this.$refs.loginForm.validate(valid => {
        if (valid) {
          this.loading = 'loading'
          this.$store.dispatch('LoginByUsername', this.loginForm).then(() => {
            setTimeout(() => {
              this.loading = 'success'
              this.$router.push({ path: '/dashboard' })
            }, 500)
            // this.showDialog = true
          }).catch(() => {
            this.loading = false
          })
        } else {
          console.log('error submit!!')
          return false
        }
      })
    },
    afterQRScan() {
      // const hash = window.location.hash.slice(1)
      // const hashObj = getQueryObject(hash)
      // const originUrl = window.location.origin
      // history.replaceState({}, '', originUrl)
      // const codeMap = {
      //   wechat: 'code',
      //   tencent: 'code'
      // }
      // const codeName = hashObj[codeMap[this.auth_type]]
      // if (!codeName) {
      //   alert('第三方登录失败')
      // } else {
      //   this.$store.dispatch('LoginByThirdparty', codeName).then(() => {
      //     this.$router.push({ path: '/' })
      //   })
      // }
    }
  },
  created() {
    // window.addEventListener('hashchange', this.afterQRScan)
  },
  destroyed() {
    // window.removeEventListener('hashchange', this.afterQRScan)
  }
}
</script>

<style rel="stylesheet/scss" lang="scss">
@import "src/styles/mixin.scss";
$bg: #f6f7fc;
$dark_gray: #889aa4;
$light_gray: #55678b;

.login-container {
  overflow: hidden;
  height: 100vh;
  background: $bg;
  background:$bg;
  color: $light_gray;
  input {
    background: transparent;
    border: 0px;
    -webkit-appearance: none;
    border-radius: 0px;
  }
  .el-input {
    display: inline-block;
    height: 47px;
    width: 85%;
  }
  .login-input {
    border: none;
  }
  .login-btn{
    display: block;
    word-spacing: 10px;
    border-radius: 20px;
    border: none;
    height: 40px;
    width: 156px;
    margin:0 auto;
    color: $light_gray;
    background: #fff;
    position: relative;
    box-shadow: 0 3px 1px -2px rgba(0,0,0,.2), 0 2px 2px 0 rgba(0,0,0,.14), 0 1px 5px 0 rgba(0,0,0,.12);
    &::after{
      position: absolute;
      top: 0;
      left: 0;
      display: block;
      content: '';
      opacity: 0;
      border:4px solid #2196f3;
    }
  }
  .login-btn-loading{
    animation: btnloading 1s linear 1;
    color: rgba(0, 0, 0, 0);
    width: 40px;
    &::after{
      opacity: 0;
      border-top-color: rgba(255, 255, 255, 0.3);
      height: 40px;
      width: 40px;
      border-radius: 100%;
      animation: 0.5s spining 0.3s linear infinite;
    }
  }
  @keyframes btnloading {
    0%{
      width: 156px;
    }
    25%{
      width: 40px;
    }
    100%{
      width: 40px;
    }
  }
  @keyframes spining {
    0% { transform: rotateZ(0deg); opacity: 1; }
  100% { transform: rotateZ(359deg); opacity: 1; }
  }
  .login-btn-success{
    color: rgba(0, 0, 0, 0);
    width: 40px;
    &::after{
      opacity: 1;
      top: 8px;
      left: 5px;
      border-bottom-color: rgba(255, 255, 255, 0.3);
      border-left-color: rgba(255, 255, 255, 0.3);
      transform: rotate(135deg);
      height: 15px;
      width: 30px;
      animation: success 0.2s ease-in-out;
    }
  }
  @keyframes success {
      0%{
        height: 25px;
        width: 40px;
        opacity: 0;
      }
      100%{
        height: 15px;
        width: 30px;
        opacity: 1;
      }
    }
  .show-pwd {
      position: absolute;
      right: 10px;
      top: 7px;
      font-size: 16px;
      color: $dark_gray;
      cursor: pointer;
      user-select:none;
    }
  .tips {
    text-align: center;
    font-size: 12px;
  }
  .svg-container {
    color: $dark_gray;
    vertical-align: middle;
    width: 30px;
    display: inline-block;
    &_login {
      font-size: 20px;
    }
  }
  .title {
    font-size: 26px;
    font-weight: 400;
    letter-spacing: 5px;
    color: $light_gray;
    margin: 0px auto 40px auto;
    text-align: center;
    font-weight: bold;
  }
  .login-form {
    position: absolute;
    z-index: 1;
    left: 0;
    right: 0;
    width: 400px;
    padding: 35px 35px 15px 35px;
    margin: 120px auto;
  }
  .el-form-item {
    border: none;
    border-bottom: 1px solid $light_gray;
    color: #454545;
  }
  .show-pwd {
    position: absolute;
    right: 10px;
    top: 7px;
    font-size: 16px;
    color: $dark_gray;
    cursor: pointer;
  }
  .third-part-login {
    display: block;
    height: 40px;
    line-height: 40px;
    text-align: center;
    text-decoration: underline;
    font-size: 12px;
  }
  .wave{
    background: url(https://wpimg.wallstcn.com/311aa8ca-5ac8-41a5-b64e-3f874d613512.svg) repeat-x;
  position: absolute;
  bottom: -20px;
  width: 6400px;
  height: 198px;
  animation: wave 5s cubic-bezier( 0.36, 0.45, 0.63, 0.53) infinite;
  transform: translate3d(0, 0, 0);
  }
  .wave:nth-of-type(2) {
  bottom: -30px;
  animation: wave 3s cubic-bezier( 0.36, 0.45, 0.63, 0.53) -.125s infinite, swell 7s ease -1.25s infinite;
  opacity: 1;
}

@keyframes wave {
  0% {
    margin-left: 0;
  }
  100% {
    margin-left: -1600px;
  }
}

@keyframes swell {
  0%, 100% {
    transform: translate3d(0,-25px,0);
  }
  50% {
    transform: translate3d(0,5px,0);
  }
}
}
</style>
