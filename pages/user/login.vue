<template>
  <div class="login">
    <el-row type="flex" class="main" align="middle" justify="center">
      <div class="form-wrapper">
        <!-- 表单头部 -->
        <el-row type="flex" justify="center" class="tabs">
          <span
            :class="{active:currentTab === index}"
            v-for="(item , index) in ['登录','注册']"
            :key="index"
            @click="handleChangeTab(index)"
          >{{item}}</span>
        </el-row>
        <!-- 登录功能组件 -->
        <loginForm  v-if ="currentTab==0"></loginForm>
        <!-- 注册功能组件 -->
        <registerForm  v-if ="currentTab==1"></registerForm>
      </div>
    </el-row>
  </div>
</template>

<script>
import loginForm from "@/components/user/loginForm.vue";
import registerForm from "@/components/user/registerForm.vue";
export default {
  data() {
    return {
      currentTab: 0
    };
  },
  components: {
    loginForm,
    registerForm
  },
  methods: {
    handleChangeTab(index) {
      this.currentTab = index;
    }
  }
};
</script>

<style lang="less" scoped>
.login {
  height: 700px;
  min-width: 1000px;
   background: url(http://157.122.54.189:9095/assets/images/th03.jfif);
}
.main {
  width: 1000px;
  height: 100%;
  margin: 0 auto;
  position: relative;
}
.form-wrapper {
  width: 400px;
  margin: 0 auto;
  background: #fff;
  box-shadow: 2px 2px 0 rgba(0, 0, 0, 0.1);
  overflow: hidden;
}
.tabs {
  span {
    display: block;
    width: 50%;
    height: 50px;
    box-sizing: border-box;
    background: #eee;
    line-height: 48px;
    text-align: center;
    cursor: pointer;
    color: #666;
    &.active {
      color: orange;
      // border-top-color: orange;
      border-top:2px solid orange;
      background: #fff;
      font-weight: bold;
    
    }
  }
}
</style>