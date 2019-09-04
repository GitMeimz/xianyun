<template>
  <div>
    <!-- 登录表单 -->
    <el-form class="form" :model="form" ref="form" :rules="rules">
      <!-- 输入框 -->
      <el-form-item class="form-item" prop="username">
        <el-input placeholder="用户名/手机" v-model="form.username"></el-input>
      </el-form-item>
      <!-- 密码框 -->
      <el-form-item class="form-item" prop="password">
        <el-input placeholder="请输入密码" type="password" v-model="form.password"></el-input>
      </el-form-item>

      <p class="form-text">
        <nuxt-link to="#">忘记密码</nuxt-link>
      </p>
      <!-- 登录按钮 -->
      <el-button class="submit" type="primary" @click="loginSubmit">登录</el-button>
    </el-form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      form: {
        username: "13800138000",
        password: "123456"
      },
      rules: {
        username: [
          {
            required: true,
            message: "请输入用户名",
            trigger: "blur"
          }
        ],
        password: [
          {
            required: true,
            message: "请输入密码",
            trigger: "blur"
          }
        ]
      }
    };
  },
  methods: {
    loginSubmit() {
      this.$refs.form.validate(valid => {
        if (valid) {
          this.$axios({
            url: "accounts/login",
            method: "POST",
            data: this.form
          })
            .then(res => {
              // ???登录成功后 把token和用户信息存储到vuex中
          this.$store.commit("user/setUserInfo", res.data)
              console.log(res);
              this.$message({
                message: "登录成功",
                type: "success"
              });
            })
            .catch(() => {
              this.$message.error("服务器异常");
            });
        }else{
            this.$message({
                message:'请填写信息',
                type:'warning'
            })
        }
      });
    }
  },
};
</script>

<style lang="less" scoped>
.form {
  padding: 25px;
}
.form-item {
  margin-bottom: 20px;
}
.form-text {
  font-size: 12px;
  color: #409eff;
  text-align: center;
  line-height: 1;
  text-align: right;
}
.submit {
  width: 100%;
  margin-top: 10px;
}
</style>

