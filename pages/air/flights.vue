<template>
  <section class="contianer">
    <el-row type="flex" justify="space-between">
      <!-- 顶部过滤列表 -->
      <div class="flights-content">
        <!-- 过滤条件 -->
        <div>
          <flightsFilters :data="cacheFlightsData" @setDataList="setDataList"></flightsFilters>
        </div>

        <!-- 航班头部布局 -->
        <div>
          <FlightsListHead></FlightsListHead>
        </div>

        <!-- 航班信息 -->
        <div>
          <flightsItem v-for="(item,index) in dataList" :key="index" :data="item"></flightsItem>
          <!-- 分页 -->
          <el-row type="flex" justify="center" style="margin-top:10px;">
            <!-- size-change：切换条数时候触发 -->
            <!-- current-change：选择页数时候触发 -->
            <!-- current-page: 当前页数 -->
            <!-- page-size：当前条数 -->
            <!-- total：总条数 -->
            <el-pagination
              @size-change="handleSizeChange"
              @current-change="handleCurrentChange"
              :current-page="pageIndex"
              :page-sizes="[5, 10, 15, 20]"
              :page-size="pageSize"
              layout="total, sizes, prev, pager, next, jumper"
              :total="flightsData.total"
            ></el-pagination>
          </el-row>
        </div>
      </div>

      <!-- 侧边栏 -->
      <div class="aside">
        <!-- 侧边栏组件 -->
        <flightsAside></flightsAside>
      </div>
    </el-row>
  </section>
</template>


<script>
import FlightsListHead from "@/components/air/FlightsListHead.vue";
import flightsItem from "@/components/air/flightsItem.vue";
import flightsFilters from "@/components/air/flightsFilters.vue";
import flightsAside from "@/components/air/flightsAside.vue";
export default {
  watch: {
    $route() {
      this.getData();
    }
  },
  data() {
    return {
      flightsData: {
        //航班总数据
        flights: [],
        info: {},
        options: {}
      },
      cacheFlightsData: {
        // 缓存一份数据，只会修改一次
        flights: [],
        info: {},
        options: {}
      },

      dataList: [],

      pageIndex: 1, //当前页数
      pageSize: 5, //显示条数
      total: 0 //总页数
    };
  },
  components: {
    FlightsListHead,
    flightsItem,
    flightsFilters,
    flightsAside
  },

  methods: {
    getData() {
      //   获取航班列表数据
      this.$axios({
        url: "airs",
        //路由url参数
        params: this.$route.query
      }).then(res => {
        console.log(res);
        //赋值给总数据
        this.flightsData = res.data;
        //赋值给缓存总数据
        this.cacheFlightsData = { ...res.data };
        this.dataList = this.flightsData.flights;
        //分页的总条数
        this.total = this.flightsData.flights.length;
        //第一页的值
        this.dataList = this.flightsData.flights.slice(0, this.pageSize);
      });
    },
    setDataList(arr) {
      // 修改总的航班列表
      this.flightsData.flights = arr;
      //重新返回第一页
      this.pageIndex = 1;
      this.dataList = this.flightsData.flights.slice(
        (this.pageIndex - 1) * this.pageSize,
        this.pageIndex * this.pageSize
      );

      //修改总条数
      this.total = arr.length;
    },
    // 每页条数切换时候触发, val是条数
    handleSizeChange(val) {
      this.pageSize = val;

      // 按照数学公式切换dataList的值
      this.dataList = this.flightsData.flights.slice(0, val);
    },

    // 页码切换时候触发, val是点击的页码
    handleCurrentChange(val) {
      this.pageIndex = val; // 当前页

      // 按照数学公式切换dataList的值
      this.dataList = this.flightsData.flights.slice(
        (this.pageIndex - 1) * this.pageSize,
        this.pageIndex * this.pageSize
      );
    }
  },
  mounted() {
    // 请求航班列表
    this.getData();
  },

  handleSizeChange(val) {
    this.pageSize = val;
    // 按照数学公式切换dataList的值
    this.dataList = this.flightsData.flights.slice(0, val);
  },
  // 页码切换时候触发, val是点击的页码
  handleCurrentChange(val) {
    this.pageIndex = val; // 当前页

    // 按照数学公式切换dataList的值
    this.dataList = this.flightsData.flights.slice(
      (this.pageIndex - 1) * this.pageSize,
      this.pageIndex * this.pageSize
    );
  }
};
</script>

<style lang="less" scoped>
.contianer {
  width: 1000px;
  margin: 20px auto;
}

.flights-content {
  width: 745px;
  font-size: 14px;
}

.aside {
  width: 240px;
  height: 100%;
}
</style>