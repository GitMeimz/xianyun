<template>
  <div class="container">
    <el-carousel :interval="5000" arrow="always">
      <el-carousel-item v-for="(item,index) in banners" :key="index">
        <div
          class="barrer"
          :style="`background:url(${$axios.defaults.baseURL}${item.url}) center center no-repeat;
                background-size:contain contain`"
        ></div>
      </el-carousel-item>
    </el-carousel>
  </div>
</template>

<script>
export default {
  data() {
    return {
      banners: [
        // {
        //   url: "http://157.122.54.189:9095/assets/images/th03.jfif"
        // },
        // {
        //   url: "http://157.122.54.189:9095/assets/images/th04.jfif"
        // }
      ]
    };
  },
  mounted(){
    console.dir(this.$axios);
    this.$axios({
      url:"/scenics/banners"
    }).then(res=>{
      console.log(res)
      const data = res.data.data;
      this.banners = data;
    })
  }
};
</script>

<style lang="less" scoped>
.el-carousel__item h3 {
  color: #475669;
  font-size: 18px;
  opacity: 0.75;
  line-height: 300px;
  margin: 0;
}

.el-carousel__item:nth-child(2n) {
  background-color: #99a9bf;
}

.el-carousel__item:nth-child(2n + 1) {
  background-color: #d3dce6;
}
/deep/ .el-carousel__container {
  height: 700px;
}
.barrer {
  width: 100%;
  height: 100%;
}
.container {
  min-width: 1000px;
  margin: 0 auto;
  position: relative;
}
</style>
