<template>
  <div class="dashboard-container">
    <el-card style="margin:20px">
      <el-button>新增面试技巧</el-button>
      <div class="search">
        <div class="left">
          <div class="searchWords">
            <span>关键词</span>
          </div>
          <div class="searchInput">
            <el-input placeholder="请输入题目编号/题干"></el-input>
          </div>
        </div>
        <div class="right">
          <el-button>清除</el-button>
          <el-button type="primary">搜索</el-button>
        </div>
      </div>

      <el-table :data="tableData" border style="width: 100%" class="table">
        <el-table-column prop="date" label="序号" width="180"></el-table-column>
        <el-table-column prop="name" label="标题" width="180"></el-table-column>
        <el-table-column prop="address" label="阅读数"></el-table-column>
        <el-table-column prop="address" label="状态"></el-table-column>
        <el-table-column prop="address" label="录入人"></el-table-column>
        <el-table-column prop="1614182228688" label="操作"></el-table-column>
      </el-table>

    </el-card>
  </div>
</template>

<script>
export default {
  name: 'ArticlesList',
  data() {
    return {
      tableData: [
        {
          date: '2016-05-02',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        }
      ]
    }
  }
}
</script>

<style scoped>
.search {
  margin-top: 20px;
  height: 30px;
  width: 100%;
  line-height: 30px;
  color: rgb(102, 102, 102);
  display: flex;
  justify-content: space-between;
}
.left,
.right {
  display: flex;
}
.left {
  background-color: rgb(243, 242, 242);
}
.searchWords {
  margin-left: 10px;
}
.el-input--medium /deep/ .el-input__inner {
  margin-left: 20px;
  height: 30px;
}
.right /deep/ .el-button--medium {
  padding-top: 7px;
}
.table{
  margin-top: 20px;
}
</style>
