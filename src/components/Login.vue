<template>
  <div class="login_container">
    <div class="login_box">
<!--      头像区域-->
      <div class="avatar_box">
        <img src="../assets/logo.png" alt="">
      </div>
<!--      登录表单区域-->
      <el-form ref="loginFormRef" :model="loginForm" :rules="loginFormRules" label-width="0px" class="login_form">
<!--        Username-->
        <el-form-item prop="username">
          <el-input  v-model="loginForm.username" prefix-icon="el-icon-s-custom"></el-input>
        </el-form-item>
<!--        Password-->
        <el-form-item prop="password">
          <el-input v-model="loginForm.password" prefix-icon="el-icon-lock" type="password"></el-input>
        </el-form-item>
<!--      Button area-->
      <el-form-item class="btns">
        <el-button type="primary" @click="login">Login</el-button>
        <el-button type="info" @click="restLoginForm">Reset</el-button>
      </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  // name: 'Login'
  data () {
    return {
      // 登录表单的数据绑定对象
      loginForm: {
        username: 'admin',
        password: '123456'
      },
      // 表单验证规则对象
      loginFormRules: {
        // 验证用户名是否合法
        username: [
          { required: true, message: 'Please input login name', trigger: 'blur' },
          { min: 3, max: 10, message: 'Length should be 3 to 10', trigger: 'blur' }
        ],
        // 验证密码是否合法
        password: [
          { required: true, message: 'Please input login password', trigger: 'blur' },
          { min: 6, max: 15, message: 'Length should be 6 to 15', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    // 点击重置按钮，重置登录表单
    restLoginForm () {
      this.$refs.loginFormRef.resetFields()
    },
    // login () {
    //   this.$refs.loginFormRef.validate((valid) => {
    //     console.log(valid)
    //   })
    // }
    login () {
      this.$refs.loginFormRef.validate(async valid => {
        if (!valid) {
          return false
        }
        const { data: res } = await this.$http.post('login', this.loginForm)
        if (res.meta.status !== 200) {
          return this.$message.error('登陸失敗')
        }
        this.$message.success('登陸成功')
        window.sessionStorage.setItem('token', res.data.token)
        // 跳轉到/home
        this.$router.push('/home')
      })
    }
  }
}
</script>

<style lang="less" scoped>
.login_container{
  background-color: #2b4b6b;
  height: 100%;
}
.login_box{
  width: 450px;
  height: 300px;
  background-color: #fff;
  border-right: 3px;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  .avatar_box{
    height: 130px;
    width: 130px;
    border: 1px solid #eee;
    border-radius: 50%;
    padding: 10px;
    box-shadow: 0 0 10px #ddd;
    position: absolute;
    left: 50%;
    transform: translate(-50%, -50%);
    background-color: #fff;
    img{
      width: 100%;
      height: 100%;
      border-radius: 50%;
      background-color: #eee;
    }
  }
}
.login_form{
  position: absolute;
  bottom: 0;
  width: 100%;
  padding: 0 20px;
  box-sizing: border-box;
}
.btns{
  display: flex;
  justify-content: flex-end;
}
</style>
