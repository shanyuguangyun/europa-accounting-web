<template>
  <body id="login_page">
    <el-form class="login_container" label-position="left" label-width="0px">
      <h3 class="login_title">系统登录</h3>
      <el-form-item>
        <el-input
          type="text"
          v-model="loginForm.username"
          auto-complete="off"
          placeholder="账号"
        ></el-input>
      </el-form-item>
      <el-form-item>
        <el-input
          type="password"
          v-model="loginForm.password"
          auto-complete="off"
          placeholder="密码"
        ></el-input>
      </el-form-item>
      <el-form-item style="width: 100%">
        <el-button
          type="primary"
          style="width: 100%; border: none"
          @click="loginSubmit"
          >登录</el-button
        >
      </el-form-item>
    </el-form>
  </body>
</template>

<script>
import { Message } from "element-ui";
import { login } from "@/api/login";
import store from "@/store";
export default {
  name: "Login",
  data() {
    return {
      loginForm: {
        username: "",
        password: "",
      },
      responseResult: [],
    };
  },
  methods: {
    loginSubmit() {
      login(this.loginForm.username, this.loginForm.password)
        .then((successResponse) => {
          if (successResponse.code === 200) {
            let data = successResponse.data;
            store.dispatch("SetToken", data.token);
            this.$router.replace({ path: "/dashboard" });
          } else if (successResponse.code === 500) {
            let message = successResponse.message;
            Message.error(message);
          }
        })
        .catch((failResponse) => {});
    },
  },
};
</script>

<style scoped>
#login_page {
  background: url("../../assets/image/bg.jpg") no-repeat;
  background-position: center;
  height: 100%;
  width: 100%;
  background-size: cover;
  position: fixed;
}
body {
  margin: 0px;
}
.login_container {
  border-radius: 15px;
  background-clip: padding-box;
  margin: 90px auto;
  width: 350px;
  padding: 35px 35px 15px 35px;
  background: #fff;
  border: 1px solid #eaeaea;
  box-shadow: 0 0 25px #cac6c6;
}

.login_title {
  margin: 0px auto 40px auto;
  text-align: center;
  color: #505458;
}
</style>

