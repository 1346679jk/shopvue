<template>
  <div class="login-container">
    <div class="login-box">
      <div class="avatar-box">
        <img src="../assets/img/logo.png" alt>
      </div>

      <el-form ref="loginFormRef" :model="loginForm" :rules="loginFormRules">
        <el-form-item prop="username">
          <el-input v-model="loginForm.username">
            <i slot="prefix" class="iconfont icon-user"></i>
          </el-input>
        </el-form-item>
        <el-form-item prop="userpwd">
          <el-input v-model="loginForm.userpwd">
            <i slot="prefix" class="iconfont icon-3702mima"></i>
          </el-input>
        </el-form-item>
        <el-row>
          <el-col :push="15">
            <el-button type="primary" @click="login()">登录</el-button>
            <el-button type="info" @click="reset()">重置</el-button>
          </el-col>
        </el-row>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      loginForm: {
        username: '',
        userpwd: ''
      },
      loginFormRules: {
        username: [{ required: true, message: '用户名必填', trigger: 'blur' }],
        userpwd: [
          {
            required: true,
            message: '密码必填',
            trigger: 'blur'
          }
        ]
      }
    }
  },
  methods: {
    reset() {
      this.$refs.loginFormRef.resetFields()
    },
    login() {
      this.$refs.loginFormRef.validate(async valid => {
        if (valid === true) {
          const { data: dt } = await this.$http.post('/login', {
            username: this.loginForm.username,
            password: this.loginForm.userpwd
          })
          console.log(dt)
          if (dt.meta.status !== 200) {
            return this.$message.error(dt.meta.msg)
          }
          window.sessionStorage.setItem('token', dt.data.token)
          this.$router.push('/home')
        }
      })
    }
  }
}
</script>

<style lang="less" scoped>
.login-container {
  background-color: #2b4b6b;
  height: 100%;
  overflow: hidden;
  .login-box {
    width: 450px;
    height: 304px;
    background-color: #fff;
    border-radius: 4px;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    .avatar-box {
      width: 130px;
      height: 130px;
      border: 1px solid #eee;
      border-radius: 50%;
      padding: 8px;
      box-shadow: 0 0 10px #eee;
      position: absolute;
      left: 50%;
      transform: translate(-50%, -50%);
      background-color: #fff;
      img {
        width: 100%;
        height: 100%;
        border-radius: 50%;
        background-color: #eee;
      }
    }
    .el-form {
      position: absolute;
      bottom: 0;
      width: 100%;
      padding: 20px;
      box-sizing: border-box;
    }
  }
}
</style>
