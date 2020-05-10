<template>
  <div class="flex">
    <br />
    <br />
    <h1>{{title}}</h1>
    <br />
    <br />
    <div class="box">
      <el-input
        style="width:20rem"
        v-model="username"
        placeholder="请输入内容"
        prefix-icon="el-icon-user-solid"
      ></el-input>
      <el-input
        style="width:20rem"
        v-model="password"
        placeholder="请输入密码"
        prefix-icon="el-icon-lock"
        show-password
      ></el-input>
      <el-button style="width:20rem" type="danger" @click="login">登录</el-button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      title: "东软云HIS医院管理系统",
      username: "",
      password: ""
    };
  },
  methods: {
    login() {
      if (this.username == "") {
        this.$message.error("请输入用户名~");
      } else if (this.password == "") {
        this.$message.error("请输入密码～");
      } else {
        if (this.username != "admin") {
          this.$message.warning("用户名错误");
        } else if (this.password != "admin") {
          this.$message.warning("密码错误");
        } else {
          this.$message.success("登录成功");
          this.$router.push("/registration");
        }
      }
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.flex {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.box {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-evenly;
  width: 35rem;
  height: 22rem;
  background: #ffffff;
  border-radius: 14px;
  box-shadow: 0px 5px 10px 0px rgba(0, 0, 0, 0.5);
}
</style>
