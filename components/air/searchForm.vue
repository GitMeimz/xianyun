<template>
  <div class="search-form">
    <el-row type="flex" class="search-tab">
      <span
        v-for="(item,index) in tabs"
        :key="index"
        :class="{active:index === currentTab}"
        @click="handleSearchTab(index)"
      >
        <i :class="item.icon"></i>
        {{item.name}}
      </span>
    </el-row>
    <!-- 表单 -->
    <el-form class="search-form-content" ref="form" label-width="80px">
      <el-form-item label="出发城市">
        <el-autocomplete
          :fetch-suggestions="queryDepartSearch"
          placeholder="请搜索出发城市"
          @select="handleDepartSelect"
          class="el-autocomplete"
          v-model="form.departCity"
        ></el-autocomplete>
      </el-form-item>

      <el-form-item label="到达城市">
        <!-- fetch-suggestions: 每次输入时候都会执行，获取搜索建议，并且显示在输入框的下拉框中 -->
        <!-- select：在下拉框中选中时候时候触发的事件 -->
        <el-autocomplete
          :fetch-suggestions="queryDestSearch"
          placeholder="请搜索到达城市"
          @select="handleDestSelect"
          class="el-autocomplete"
          v-model="form.destCity"
        ></el-autocomplete>
      </el-form-item>

      <el-form-item label="出发时间">
        <!-- change：选中日期时候触发 -->
        <el-date-picker
          type="date"
          placeholder="请选择日期"
          style="width: 100%;"
          @change="handleDate"
          v-model="form.departDate"
        ></el-date-picker>
      </el-form-item>

      <el-form-item label>
        <el-button style="width:100%;" type="primary" icon="el-icon-search" @click="handleSubmit">搜索</el-button>
      </el-form-item>
      <div class="reverse">
        <span @click="handleReverse">换</span>
      </div>
    </el-form>
  </div>
</template>

<script>
// import moment from "moment";
export default {
  data() {
    return {
      tabs: [
        { icon: "iconfont icondancheng", name: "单程" },
        { icon: "iconfont iconshuangxiang", name: "往返" }
      ],
      currentTab: 0,
      form: {
        departCity: "",
        departCode: "", //出发城市代码
        destCity: "",
        destCode: "", //到达城市代码
        departDate: ""
      }
    };
  },
  methods: {
    handleDestSelect() {},

    handleDepartSelect() {},

 
    handleSearchTab(index) {
      if (index === 1) {
        this.$confirm("目前暂不支持往返，请使用单程选票！", "提示", {
          confirmButtonText: "确定",
          showCancelButton: false,
          type: "warning"
        });
      }
    },
    queryDepartSearch(value, cb) {
      if (!value) {
        cb([]);
        return;
      }
      this.$axios({
        url: "/airs/city",
        params: {
          name: value
        }
      }).then(res => {
        console.log(res);
        const { data } = res.data;
        const newData = [];
        data.forEach(v => {
          v.value = v.name.replace("市", "");
          newData.push(v);
        });
        // 默认选中第一个
        this.form.departCity = newData[0].value;
        this.form.departCode = newData[0].sort;
        cb(newData);
      });
    },
    queryDestSearch(value, cb) {
      if (!value) {
        // 传递空数组不会出现下拉框
        cb([]);
        return;
      }

      // 根据用户的输入请求建议城市
      this.$axios({
        url: "/airs/city",
        // get参数
        params: {
          // 输入框的关键字
          name: value
        }
      }).then(res => {
        // 数组
        const { data } = res.data;

        // 给数组中每个对象添加value属性
        const newData = [];
        data.forEach(v => {
          // 添加value属性
          v.value = v.name.replace("市", "");
          // 把带有value属性的对象添加到新数组中
          newData.push(v);
        });

        // 默认选中第一个
        this.form.destCity = newData[0].value;
        this.form.destCode = newData[0].sort;

        //显示到下拉列表中
        cb(newData);
      });
    },
    handleDate(value) {
      //   this.form.departDate = moment(value).format(`YYYY-MM-DD`);
    },
    handleSubmit() {
      console.log(this.form);
      const rules = {
        depart: {
          value: this.form.departCity,
          message: "请选择出发城市"
        },
        dest: {
          value: this.form.destCity,
          message: "请选择到达城市"
        },
        departDate: {
          value: this.form.departDate,
          message: "请选择出发时间"
        }
      };
    },
       handleReverse() {
         const {departCity, departCode, destCity, destCode} = this.form;

            // 交叉赋值
            this.form.departCity = destCity;
            this.form.departCode = destCode;

            this.form.destCity = departCity;
            this.form.destCode = departCode;
    },
  }
};
</script>

<style lang="less" scoped>
.search-form {
  border: 1px #ddd solid;
  border-top: none;
  width: 360px;
  height: 350px;
  box-sizing: border-box;
}

.search-tab {
  span {
    display: block;
    flex: 1;
    text-align: center;
    height: 48px;
    line-height: 42px;
    box-sizing: border-box;
    border-top: 3px #eee solid;
    background: #eee;
  }

  .active {
    border-top-color: orange;
    background: #fff;
  }

  i {
    margin-right: 5px;
    font-size: 18px;

    &:first-child {
      font-size: 16px;
    }
  }
}

.search-form-content {
  padding: 15px 50px 15px 15px;
  position: relative;

  .el-autocomplete {
    width: 100%;
  }
}

.reverse {
  position: absolute;
  top: 35px;
  right: 15px;

  &:after,
  &:before {
    display: block;
    content: "";
    position: absolute;
    left: -35px;
    width: 25px;
    height: 1px;
    background: #ccc;
  }

  &:after {
    top: 0;
  }

  &:before {
    top: 60px;
  }

  span {
    display: block;
    position: absolute;
    top: 20px;
    right: 0;
    font-size: 12px;
    background: #999;
    color: #fff;
    width: 20px;
    height: 20px;
    line-height: 18px;
    text-align: center;
    border-radius: 2px;
    cursor: pointer;

    &:after,
    &:before {
      display: block;
      content: "";
      position: absolute;
      left: 10px;
      width: 1px;
      height: 20px;
      background: #ccc;
    }

    &:after {
      top: -20px;
    }

    &:before {
      top: 20px;
    }
  }
}
</style>