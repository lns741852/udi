<template>
  <div class="board-center">
    <h3>瑕疵包補輸報表</h3>
    <!--卡片區塊-->
    <el-card class="box-card">
      <el-form :inline="true" :model="formInline" class="demo-form-inline">
        <el-form-item label="開始時間">
          <el-date-picker
            v-model="queryInfo.startTime"
            type="date"
            placeholder="選擇日期"
          >
          </el-date-picker>
        </el-form-item>
        <el-form-item label="結束時間">
          <el-date-picker
            v-model="queryInfo.endTime"
            aria-hidden="true"
            type="date"
            placeholder="選擇日期"
          >
          </el-date-picker>
        </el-form-item>
        <el-button class="edit_button" @click="getReportList">查詢</el-button>
      </el-form>
    </el-card>
  </div>
</template>


<script>
export default {
  data() {
    return {
      queryInfo: {
        startTime: new Date(
          new Date(new Date().toLocaleDateString()).getTime()
        ), //0點
        endTime: new Date(
          new Date(new Date().toLocaleDateString()).getTime() +
            24 * 60 * 60 * 1000 -
            1
        ), //23:59點
      },
      reportList: [],
      depnoList: [],
    };
  },
  created() {
  },
  methods: {
    /**列表查詢 */
    getReportList() {

      //去除空資料
      for (let i in this.queryInfo) {
        if (this.queryInfo[i] === "" || this.queryInfo[i] === null) {
          this.queryInfo[i] = null; //速度比delete快
        }
      }

      this.$axios.get("/kpi/01", this.queryInfo).then((res) => {
        this.reportList = res.data.data;
      });
    },

  },
};
</script>

<style lang="less" scope>
div {
  .board-center {
    margin: 0 auto;
    width: 80%;
  }
  .el-link {
    font-size: 20px;
  }
}
.el-calendar-table .el-calendar-day {
  height: 30px;
}
</style>