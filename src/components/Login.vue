<template>
    <div class="login_container">
      <div class="login_box">
        <!--   头像区域     -->
        <div class="avatar_box">
          <img src="../assets/logo.png" />
        </div>

        <!--   表单区域     -->
        <el-form ref="loginRef" :model="loginForm" :rules="loginRules" label-width="0px" class="login_form">
          <!-- 用户名 -->
          <el-form-item prop="username">
            <el-input v-model="loginForm.username" prefix-icon="el-icon-s-custom"></el-input>
          </el-form-item>
          <!-- 密码 -->
          <el-form-item prop="password">
            <el-input v-model="loginForm.password" prefix-icon="el-icon-lock" type="password"></el-input>
          </el-form-item>
          <!-- 登录 -->
          <el-form-item class="btns">
            <el-button type="primary" @click="submitForm">登录</el-button>
            <el-button type="info" @click="resetForm">重置</el-button>
          </el-form-item>
        </el-form>
      </div>
    </div>
</template>

<script>
    export default {
        name: "Login",
        data() {
            return {
                loginForm: {
                    username: 'admin',
                    password: '123456'
                },
                loginRules: {
                    username: [
                        {required: true, message: '请输入用户名', trigger: 'blur'},
                        {min: 3, max: 20, message: '长度在 3 到 20 个字符', trigger: 'blur'}
                    ],
                    password: [
                        {required: true, message: '请输入密码', trigger: 'blur'},
                        {min: 6, max: 8, message: '长度在 6 到 8 个字符', trigger: 'blur'}
                    ]
                }
            }
        },
        methods: {
            submitForm() {
                this.$refs.loginRef.validate( async valid => {
                    if(!valid) return;
                    const {data: res} = await this.$http.post('login', this.loginForm);
                    if(res.meta.status !== 200) return this.$message.error(res.meta.msg);
                    // 存放token
                    window.sessionStorage.setItem('token', res.data.token);
                    this.$router.push('/home');
                });
            },
            resetForm() {
                this.$refs.loginRef.resetFields();
            }
        }
    }
</script>

<style lang="less" scoped>
  .login_container {
    background-color: #2b4b6b;
    height: 100%;
  }

  .login_box {
    width: 450px;
    height: 300px;
    background-color: #fff;
    border-radius: 3px;
    position: absolute;
    top: 0;
    left: 0;
    bottom: 0;
    right: 0;
    margin: auto;

    .avatar_box {
      width: 130px;
      height: 130px;
      border: 1px solid #eee;
      border-radius: 50%;
      box-shadow: 0 0 10px #ddd;
      padding: 10px;
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
  }

  .login_form {
    position: absolute;
    bottom: 0;
    width: 100%;
    padding: 0 20px;
    box-sizing: border-box;
  }

  .btns {
    display: flex;
    justify-content: flex-end;
  }
</style>
