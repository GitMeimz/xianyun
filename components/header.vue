<template>
  <div class="container">
    <!-- justify-content:space-between; 左右贴边对齐 -->
    <!-- 文档： https://element.eleme.cn/#/zh-CN/component/layout#dui-qi-fang-shi -->
    <el-row type="flex" class="main" justify="space-between">
      <!-- logo -->
      <div class="logo">
        <img src="http://157.122.54.189:9093/images/logo.jpg" alt />
      </div>

      <el-row type="flex" class="navs">
        <!-- nuxt-link的作用和使用方式和router-link -->
        <nuxt-link to="/">首页</nuxt-link>
        <nuxt-link to="/post">旅游攻略</nuxt-link>
        <nuxt-link to="/hotel">酒店</nuxt-link>
        <nuxt-link to="/air">国内机票</nuxt-link>
      </el-row>

      <!-- 登录跳转    -->
      <div v-if="!$store.state.user.userInfo.token">
         <!-- <div v-if="false"> -->
        <nuxt-link to="/user/login">注册/登录</nuxt-link>
      </div>
      <div v-else>
        <el-dropdown>
          <span class="el-dropdown-link ">
            <img :src="$axios.defaults.baseURL+$store.state.user.userInfo.user.defaultAvatar" alt />
            <span>{{$store.state.user.userInfo.user.nickname}}</span>
            <i class="el-icon-arrow-down el-icon--right"></i>
          </span>
          <el-dropdown-menu slot="dropdown">
            <el-dropdown-item>个人中心</el-dropdown-item>
            <el-dropdown-item @click.native="handleLogout">退出</el-dropdown-item>
          </el-dropdown-menu>
        </el-dropdown>
      </div>
    </el-row>
  </div>
</template>

<script>
export default {
  methods:{
    handleLogout(){
      const {commit} = this.$store;
      commit('user/clearUserInfo');
      this.$message({
        message:'退出成功',
        type:'success'
      })
    }
  },
    mounted(){
        console.log(this.$store.state.user.userInfo)
    }
};
</script>

<style scoped lang="less">
.container {
  width: 100%;
  height: 60px;
  line-height: 60px;
  border-bottom: 1px #ddd solid;
  box-shadow: 0 2px 2px #ddd;
}

.main {
  width: 1000px;
  margin: 0 auto;
}

.navs {
  flex: 1;
  margin-left: 10px;

  a {
    display: block;
    height: 60px;
    padding: 0 20px;
    box-sizing: border-box;

    &:hover {
      color: #409eff;
      border-bottom: 5px #409eff solid;
    }
  }
  .nuxt-link-exact-active {
    background: #409eff;
    color: #fff;

    &:hover {
      color: #fff;
    }
  }
}

.logo {
  padding-top: 9px;
  img {
    width: 156px;
    height: 42px;
    display: block;
  }
}
.el-dropdown-link{
  cursor: pointer;
  img{
  width: 36px;
  height: 36px;
  border-radius: 50%;
  box-sizing: border-box;
 vertical-align: middle;
 border:2px #fff solid;
 &:hover{
   border:2px #409eff solid;
 }
}
}
</style>