<template>
  <div>
    <el-form class="form" :model="form" ref="form" :rules="rules">
      <el-form-item class="form-item" prop="username">
        <el-input type="text" autocomplete="off" placeholder="用户名手机" v-model="form.username"></el-input>
      </el-form-item>
      <el-form-item class="form-item" prop="captcha">
        <el-input
          type="text"
          autocomplete="off"
          placeholder="验证码"
          class="input"
          v-model="form.captcha"
        >
          <el-button slot="append" @click="send">发送验证码</el-button>
        </el-input>
      </el-form-item>

      <el-form-item class="form-item" prop="nickname">
        <el-input type="text" autocomplete="off" placeholder="你的名字" v-model="form.nickname"></el-input>
      </el-form-item>
      <el-form-item class="form-item" prop="password">
        <el-input type="password" autocomplete="off" placeholder="密码" v-model="form.password"></el-input>
      </el-form-item>
      <el-form-item class="form-item" prop="checkPassword">
        <el-input
          type="password"
          autocomplete="off"
          placeholder="确认密码"
          v-model="form.checkPassword"
        ></el-input>
      </el-form-item>
      <el-form-item class="form-item">
        <el-button type="primary" class="submit" @click="handleRegSubmit">注册</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      form: {
        username: "",
        password: "",
        nickname: "",
        checkPassword: "",
        captcha: ""
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
        ],
        nickname: [
          {
            required: true,
            message: "请输入昵称",
            trigger: "blur"
          }
        ],
        checkPassword: [
          {
            required: true,
            message: "请确认密码",
            trigger: "blur"
          }
        ],
        captcha: [
          {
            required: true,
            message: "请输入验证码",
            trigger: "blur"
          }
        ]
      }
    };
  },
  methods: {
    //   发送验证码
    send() {
      if (!this.form.username) {
        this.$confirm("手机号码不能为空", "提示", {
          confirmButtonText: "确定",
          showCancelButton: false,
          type: "warning"
        });
        return;
      }
      if (this.form.username.length !== 11) {
        this.$confirm("手机号码格式错误", "提示", {
          confirmButtonText: "确定",
          showCancelButton: false,
          type: "warning"
        });
        return;
      }
      this.$axios({
        url: `captchas`,
        method: "POST",
        data: { tel: this.form.username }
      }).then(res => {
        console.log(res);
        const { code } = res.data;
        this.$confirm(`验证码：${code}`, "提示", {
          confirmButtonText: "确定",
          showCancelButton: false,
          type: "warning"
        });
      });
    },
    //   注册
    handleRegSubmit() {
      this.$refs["form"].validate(valid => {
        if (valid) {
            const{checkPassword, ...props} = this.form;
          this.$axios({
            url: "/accounts/register",
            method: "POST",
            data: props
          })
          .then(res=>{
               this.$store.commit("user/setUserInfo", res.data)
          })
        }
      });
    }
  }
};
</script>

<style lang="less" scoped>
.form {
  padding: 25px;
}
.form-item {
  margin-bottom: 20px;
}
.submit {
  width: 100%;
  margin-top: 10px;
}
</style>