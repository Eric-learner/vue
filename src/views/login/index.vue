<template>
  <div class="login-container">
    <el-form ref="loginForm" :model="loginForm" :rules="loginRules" class="login-form" autocomplete="on" label-position="left">

      <div class="title-container">
        <h3 style="color:#FFFFFF" class="title">欢迎登录</h3>
      </div>

      <el-form-item prop="username">
        <span class="svg-container">
          <svg-icon icon-class="user" />
        </span>
        <el-input
          ref="username"
          v-model="loginForm.username"
          placeholder="Username"
          name="username"
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
            v-model="loginForm.password"
            :type="passwordType"
            placeholder="Password"
            name="password"
            tabindex="2"
            autocomplete="on"
            @keyup.native="checkCapslock"
            @blur="capsTooltip = false"
          />
          <span class="show-pwd" @click="showPwd">
            <svg-icon :icon-class="passwordType === 'password' ? 'eye' : 'eye-open'" />
          </span>
        </el-form-item>
      </el-tooltip>

      <el-button :loading="loading" type="primary" style="width:100%;margin-top:10px;margin-bottom:30px;" @click.native.prevent="handleLogin">登录</el-button>
<!--      <el-button :loading="loading" type="primary" style="width:100%;margin-top:10px;margin-bottom:30px;" @click.native.prevent="handleLogin">后的</el-button>-->
<!--      <el-button :loading="loading" type="primary" style="width:50%;margin-top:10px;margin-bottom:60px;" @click.native.prevent="handleLogin">登录</el-button>-->
<!--      <el-button size="mini" icon="el-icon-refresh-right" type="goon" >继续跟进</el-button>-->
      <div align="center">
        <el-button icon="el-icon-refresh-right" type="warning" style="width:100%;margin-top:10px;margin-bottom:30px;" @click="register">没有账号？快去注册</el-button>
      </div>
<!--      <div class="sign-switch">-->
<!--        &lt;!&ndash; 切换注册链接 &ndash;&gt;-->
<!--        <div class="sign-link">-->
<!--          <a @click="sign">没有账号？快去注册</a>-->
<!--        </div>-->
<!--      </div>-->
    </el-form>

  </div>
</template>

<script>
// import { validUsername } from '@/utils/validate'
import axios from 'axios'
import Vue from 'vue'
Vue.prototype.$axios = axios
export default {
  name: 'Login',
  data() {
    const validateUsername = (rule, value, callback) => {
      if (value.length < 11) {
        callback(new Error('Please enter the correct phone number'))
      } else {
        callback()
      }
    }
    const validatePassword = (rule, value, callback) => {
      if (value.length < 6) {
        callback(new Error('The password can not be less than 6 digits'))
      } else {
        callback()
      }
    }
    return {
      loginForm: {
        username: '',
        password: ''
      },
      loginRules: {
        username: [{ required: true, trigger: 'blur', validator: validateUsername }],
        password: [{ required: true, trigger: 'blur', validator: validatePassword }]
      },
      passwordType: 'password',
      capsTooltip: false,
      loading: false,
      redirect: undefined,
      otherQuery: {},
      access_token: ''
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
  mounted() {
    if (this.loginForm.username === '') {
      this.$refs.username.focus()
    } else if (this.loginForm.password === '') {
      this.$refs.password.focus()
    }
  },
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
    register() {
      // // 跳转到上一次浏览的页面
      // this.$router.go(-1)
      //
      // 指定跳转的地址
      // this.$router.replace('/register')
      // this.$router.push({ path: this.redirect || '/', query: this.otherQuery })
      // //
      // // 指定跳转的路由的名字下
      // this.$router.replace({ name: 'Register' }) // http://localhost:9527/#/register
      // this.$router.push('/' + 'register') // 路由改了
      // this.$router.push('/' + 'register')
      // // 通过push进行跳转
      // this.$router.push('Register')// homepage为要跳转的页面
      this.$router.push('/' + 'register') // 路由改了
    },
    // register() {
    //   this.$refs.loginForm.validate(valid => {
    //     this.loading = true
    //     this.$store.dispatch('user/login', this.loginForm)
    //       .then(() => {
    //         // this.$router.push({ path: this.redirect || '/', query: this.otherQuery })
    //         this.$router.push('/' + 'register') // 路由改了
    //
    //         this.loading = false
    //       })
    //   })
    // },
    handleLogin() {
      // console.log(this.loginForm.)
      // this.$refs.loginForm.validate(valid => {
      //   if (valid) {
      //     this.loading = true
      //     this.$store.dispatch('user/login', this.loginForm)
      //       .then(() => {
      //         // this.$router.push({ path: this.redirect || '/', query: this.otherQuery })
      // this.$router.push('/' + 'dashboard') // 路由改了
      // localStorage.setItem()
      //
      //         this.loading = false
      //       })
      //       .catch(() => {
      //         this.loading = false
      //       })
      //   } else {
      //     console.log('error submit!!')
      //     return false
      //   }
      // })
      var that = this
      this.loading = true
      console.log(this.loginForm.username + ' ' + this.loginForm.password + ' ' + this.GRANT_TYPE + ' ' + this.CLIENT_ID + ' ' + this.CLIENT_SECRET + ' ' + this.SCOPE)
      this.$axios.post(this.HTTP_URL + '/authorizations', { username: this.loginForm.username, password: this.loginForm.password, grant_type: this.GRANT_TYPE, client_id: this.CLIENT_ID, client_secret: this.CLIENT_SECRET, scope: this.SCOPE })
        .then(function(response) {
          console.log(response)
          that.access_token = response.data.access_token
          // this.picurl = response.captcha_image_content
          // that.picurl = response.captcha_image_content
          // console.log(that.picurl)
          that.$axios.get(that.HTTP_URL + '/user',
            {
              headers: { 'Authorization': 'Bearer ' + that.access_token }
              // timeout: 1000,  //跟params并列，都在一个{}里面
              // baseURL:'xxxx'
            })
            .then(function(response) {
              console.log(response)
              window.localStorage.setItem('id', response.data.id)
              window.localStorage.setItem('phone', response.data.phone)
              // window.localStorage.getItem('key');
              // window.localStorage.removeItem('key');
              // that.access_token = response.data.access_token
              // this.picurl = response.captcha_image_content
              // that.picurl = response.captcha_image_content
              // console.log(that.picurl)
              that.$router.push('/' + 'dashboard') // 路由改了
            })
            .catch(function(response) {
              console.log(that.access_token)
              that.$alert('错误', 'get问题', {
                confirmButtonText: '确定'
              })
              console.log(response)
            })
        })
        .catch(function(response) {
          that.$alert('错误', '服务器未响应', {
            confirmButtonText: '确定'
          })
          console.log(response)
        })
    },
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

  //background: url("http://www.5guav.com.cn/wp-content/uploads/2020/07/%E5%9B%BE%E7%89%871.jpg") no-repeat;
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

  .thirdparty-button {
    position: absolute;
    right: 0;
    bottom: 6px;
  }

  @media only screen and (max-width: 470px) {
    .thirdparty-button {
      display: none;
    }
  }
}
</style>
