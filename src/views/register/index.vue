<template>
  <div class="login-container">
    <el-form ref="loginForm" :model="registerForm" :rules="registerRules" class="login-form" autocomplete="on" label-position="left">

      <div class="title-container">
        <h3 style="color:#FFFFFF" class="title">欢迎注册</h3>
      </div>

      <el-form-item prop="username">
        <span class="svg-container">
          <svg-icon icon-class="user" />
        </span>
        <el-input
          ref="username"
          v-model="registerForm.username"
          placeholder="输入手机号"
          name="username"
          type="text"
          tabindex="1"
          autocomplete="on"
        />
      </el-form-item>

      <el-button type="primary" @click="getPicMessage">获取图片验证码</el-button>

      <img v-if="picurl" :src="picurl" width="150" height="60">

      <el-form-item prop="picnum">
        <span class="svg-container">
          <svg-icon icon-class="pic" />
        </span>
        <el-input
          ref="picnum"
          v-model="registerForm.picnum"
          placeholder="图片验证码"
          name="picnum"
          type="text"
          tabindex="1"
          autocomplete="on"
        />
      </el-form-item>
      <el-button type="primary" :disabled="disable" :class="{ codeGeting:isGeting }" @click="getMessage">{{ getCode }}</el-button>

      <!--      <el-button type="primary" @click="getMessage">获取短信验证码</el-button>-->
      <el-form-item prop="message">
        <span class="svg-container">
          <svg-icon icon-class="message" />
        </span>
        <el-input
          ref="message"
          v-model="registerForm.message"
          placeholder="短信验证码"
          name="message"
          type="text"
          tabindex="1"
          autocomplete="on"
        />
      </el-form-item>
      <el-tooltip v-model="capsTooltip" content="Caps lock is On" placement="right" manual>
        <el-form-item prop="password">
          <span class="svg-container">
            <svg-icon icon-class="password" />
          </span>
          <el-input
            :key="passwordType"
            ref="password"
            v-model="registerForm.password"
            :type="passwordType"
            placeholder="Password"
            name="password"
            tabindex="2"
            autocomplete="on"
            @keyup.native="checkCapslock"
            @blur="capsTooltip = false"
            @keyup.enter.native="handleLogin"
          />
          <span class="show-pwd" @click="showPwd">
            <svg-icon :icon-class="passwordType === 'password' ? 'eye' : 'eye-open'" />
          </span>
        </el-form-item>
      </el-tooltip>

      <el-button :loading="loading" type="primary" style="width:100%;margin-top:10px;margin-bottom:30px;" @click.native.prevent="register">注册完成</el-button>

    </el-form>

  </div>
</template>

<script>
import axios from 'axios'
import Vue from 'vue'
Vue.prototype.$axios = axios
// var picurl = ''
export default {
  name: 'Register',

  data() {
    const validateUsername = (rule, value, callback) => {
      if (value.length < 11) {
        callback(new Error('Please enter the correct phone number'))
      } else {
        callback()
      }
    }
    const validatePicnum = (rule, value, callback) => {
      if (value.length < 4) {
        callback(new Error('Please enter the correct num'))
      } else {
        callback()
      }
    }
    // const validatePicNum = (rule, value, callback) => {
    //   if (!validUsername(value)) {
    //     callback(new Error('Please enter the correct user name'))
    //   } else {
    //     callback()
    //   }
    // }
    const validatePassword = (rule, value, callback) => {
      if (value.length < 6) {
        callback(new Error('The password can not be less than 6 digits'))
      } else {
        callback()
      }
    }
    const validateMessage = (rule, value, callback) => {
      if (value.length < 4) {
        callback(new Error('The message can not be less than 4 digits'))
      } else {
        callback()
      }
    }
    return {
      registerForm: {
        username: '',
        picnum: '',
        message: '',
        password: ''

      },
      registerRules: {
        username: [{ required: true, trigger: 'blur', validator: validateUsername }],
        picnum: [{ required: true, trigger: 'blur', validator: validatePicnum }],
        message: [{ required: true, trigger: 'blur', validator: validateMessage }],
        password: [{ required: true, trigger: 'blur', validator: validatePassword }]
      },
      passwordType: 'password',
      capsTooltip: false,
      loading: false,
      redirect: undefined,
      otherQuery: {},
      picurl: '',
      captcha_code_key: '',
      verification_code_key: '',
      captcha_code: '',
      getCode: '获取短信验证码',
      isGeting: false,
      count: 120,
      disable: false

      // messageCode: ''
    }
  },
  watch: {
    $route: {
      handler: function(route) {
        const query = route.query
        if (query) {
          this.redirect = query.redirect
          this.otherQuery = this.getOtherQuery(query)
        }
      },
      immediate: true
    }
  },
  // mounted() {
  //   if (this.loginForm.username === '') {
  //     this.$refs.username.focus()
  //   } else if (this.loginForm.password === '') {
  //     this.$refs.password.focus()
  //   }
  // },
  methods: {
    checkCapslock(e) {
      const { key } = e
      this.capsTooltip = key && key.length === 1 && (key >= 'A' && key <= 'Z')
    },
    showPwd() {
      if (this.passwordType === 'password') {
        this.passwordType = ''
      } else {
        this.passwordType = 'password'
      }
      this.$nextTick(() => {
        this.$refs.password.focus()
      })
    },
    // 获取验证码
    // getcode() {
    //   const randomLetterAndNums = Math.random()
    //     .toString(36)
    //     .substr(2)
    //   // eslint-disable-next-line no-undef
    //   request
    //     .get(`/api/security/captcha/image/${randomLetterAndNums}`, {
    //       responseType: 'blob'
    //     })
    //     .then((res) => {
    //       this.images = window.URL.createObjectURL(res)
    //     })
    //     .catch(() => {})
    // },

    getPicMessage() {
      var that = this
      // 考虑更多情况
      if (this.registerForm.username.length < 11) {
        // eslint-disable-next-line no-undef
        callback(new Error('Please enter the correct phone number'))
      } else {
        console.log(this.registerForm.username)

        this.$axios.post(this.HTTP_URL + '/captchas', { phone: this.registerForm.username })
          .then(function(response) {
            console.log(response)
            that.picurl = response.data.captcha_image_content
            that.captcha_code_key = response.data.captcha_code_key
            that.captcha_code = response.data.captcha_code_value_ori
            // console.log(that.picurl)
          })
          .catch(function(response) {
            console.log(response)
          }
          )
      }
    },
    getMessage() {
      var that = this

      if (this.registerForm.picnum.length !== 4) {
        this.$alert('验证码错误', '请重新输入四位验证码', {
          confirmButtonText: '确定'
        })
      } else {
        console.log(this.registerForm.username)
        console.log('{{' + this.captcha_code_key + '}}' + '{{' + this.registerForm.picnum + '}}')
        //
        this.$axios.post(this.HTTP_URL + '/verificationCodes', { captcha_code_key: this.captcha_code_key, captcha_code_value: this.registerForm.picnum })
          .then(function(response) {
            if (response.status === 201 || response.status === 200) {
              console.log(response)
              that.verification_code_key = response.data.verification_code_key
              that.$alert('短信验证码已发送', '请输入', {
                confirmButtonText: '确定'
              })
              var countDown = setInterval(() => {
                if (that.count < 1) {
                  that.isGeting = false
                  that.disable = false
                  that.getCode = '获取短信验证码'
                  that.count = 120
                  clearInterval(countDown)
                } else {
                  that.isGeting = true
                  that.disable = true
                  that.getCode = that.count-- + 's后重发'
                }
              }, 1000)
            }
          })
          .catch(function(response) {
            console.log(response)
            that.$alert('图片验证码错误', '请重新输入', {
              confirmButtonText: '确定'
            })
            // 重新请求新的图片验证码
            that.$axios.post(that.HTTP_URL + '/captchas', { phone: that.registerForm.username })
              .then(function(response) {
                console.log(response)
                that.picurl = response.data.captcha_image_content
                that.captcha_code_key = response.data.captcha_code_key
                that.captcha_code = response.data.captcha_code_value_ori
                // console.log(that.picurl)
              })
              .catch(function(response) {
                console.log(response)
              }
              )
          })
      }
    },

    register() {
      var that = this
      this.$refs.loginForm.validate(valid => {
        if (valid) {
          console.log(this.registerForm)
          this.loading = true
          console.log(this.verification_code_key + this.registerForm.message + this.registerForm.password)
          this.$axios.post(this.HTTP_URL + '/users', { verification_code_key: this.verification_code_key, verification_code_value: this.registerForm.message, password: this.registerForm.password })
            .then(function(response) {
              console.log(response)
              that.verification_code_key = response.data.verification_code_key

              that.$alert('注册成功', '请登录', {
                confirmButtonText: '确定'
              })
              that.$router.push('/' + 'login') // 路由改了
            })
            .catch(function(response) {
              that.$alert('请重试', '错误', {
                confirmButtonText: '确定'
              })
              console.log(response)
            })
        } else {
          console.log('error submit!!')

          return false
        }
      })
    },
    // getVerifyCode() {
    //   var countDown = setInterval(() => {
    //     if (this.count < 1) {
    //       this.isGeting = false
    //       this.disable = false
    //       this.getCode = '获取短信验证码'
    //       this.count = 120
    //       clearInterval(countDown)
    //     } else {
    //       this.isGeting = true
    //       this.disable = true
    //       this.getCode = this.count-- + 's后重发'
    //     }
    //   }, 1000)
    // },
    getOtherQuery(query) {
      return Object.keys(query).reduce((acc, cur) => {
        if (cur !== 'redirect') {
          acc[cur] = query[cur]
        }
        return acc
      }, {})
    }
  }
}
</script>

<style lang="scss">
/* 修复input 背景不协调 和光标变色 */
/* Detail see https://github.com/PanJiaChen/vue-element-admin/pull/927 */

$bg:#283443;
$light_gray:#fff;
$cursor: #fff;

@supports (-webkit-mask: none) and (not (cater-color: $cursor)) {
  .login-container .el-input input {
    color: $cursor;
  }
}

/* reset element-ui css */
.login-container {

  //background: url("https://img2.baidu.com/it/u=3150648368,3300870086&fm=253&fmt=auto&app=138&f=JPEG?w=719&h=500") no-repeat;
  //background: url("http://www.5guav.com.cn/wp-content/uploads/2020/07/%E5%9B%BE%E7%89%871.jpg") no-repeat;
  //background-size: cover;
  .el-input {
    display: inline-block;
    height: 47px;
    width: 85%;

    input {
      background: transparent;
      border: 0px;
      -webkit-appearance: none;
      border-radius: 0px;
      padding: 12px 5px 12px 15px;
      color: $light_gray;
      height: 47px;
      caret-color: $cursor;
      &:-webkit-autofill {
        box-shadow: 0 0 0px 1000px $bg inset !important;
        -webkit-text-fill-color: $cursor !important;
      }
    }
  }

  .el-form-item {
    border: 1px solid rgba(255, 255, 255, 0.1);
    background: rgba(0, 0, 0, 0.1);
    border-radius: 5px;
    color: #454545;
  }
}
</style>

<style lang="scss" scoped>
$bg:#2d3a4b;
$dark_gray:#889aa4;
$light_gray:#eee;

.login-container {
  min-height: 100%;
  width: 100%;
  background-color: $bg;
  overflow: hidden;

  .login-form {
    position: relative;
    width: 520px;
    max-width: 100%;
    padding: 160px 35px 0;
    margin: 0 auto;
    overflow: hidden;
  }

  .tips {
    font-size: 14px;
    color: #fff;
    margin-bottom: 10px;

    span {
      &:first-of-type {
        margin-right: 16px;
      }
    }
  }

  .svg-container {
    padding: 6px 5px 6px 15px;
    color: $dark_gray;
    vertical-align: middle;
    width: 30px;
    display: inline-block;
  }

  .title-container {
    position: relative;

    .title {
      font-size: 26px;
      color: $light_gray;
      margin: 0px auto 40px auto;
      text-align: center;
      font-weight: bold;
    }
  }

  .show-pwd {
    position: absolute;
    right: 10px;
    top: 7px;
    font-size: 16px;
    color: $dark_gray;
    cursor: pointer;
    user-select: none;
  }
  //.picClass{
  //  font-size: 26px;
  //  right: 10px;
  //  top: 7px;
  //  color: $light_gray;
  //  //margin: 0px auto 40px auto;
  //  text-align: center;
  //  font-weight: bold;
  //}
  .thirdparty-button {
    position: absolute;
    right: 0;
    bottom: 6px;
  }
  .codeGeting{
    background: #cdcdcd;
    border-color: #cdcdcd;
  }
  @media only screen and (max-width: 470px) {
    .thirdparty-button {
      display: none;
    }
  }
}
</style>
