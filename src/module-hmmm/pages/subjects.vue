<template>
  <div class="dashboard-container">
    <el-card>
      <el-row>
        <el-button style="background:#eee">新增学科</el-button>
      </el-row>
      <el-row class="header">
        <div class="left">
          <span>学科名称：</span>
          <el-input style="width:250px" placeholder="请输入" v-model="subjectName" />
        </div>
        <div class="right">
          <el-button @click="subjectName=''">清除</el-button>
          <el-button type="primary" @click="getSubjectsList">搜索</el-button>
        </div>
      </el-row>
      <el-table :data="tableData" border>
        <el-table-column type="index" label="序号" width="120" align="center"></el-table-column>
        <el-table-column prop="subjectName" label="学科名称" width="200"></el-table-column>
        <el-table-column prop="username" label="创建者" width="200"></el-table-column>
        <el-table-column label="创建日期" width="200">
          <span slot-scope="aa">{{aa.row.addDate | parseTimeByString}}</span>
        </el-table-column>
        <el-table-column
          prop="isFrontDisplay"
          label="前台是否显示"
          width="150"
          align="center"
          :formatter="orIsFrontDisplay"
        ></el-table-column>
        <el-table-column prop="twoLevelDirectory" label="二级目录" width="150"></el-table-column>
        <el-table-column prop="tags" label="标签" width="150"></el-table-column>
        <el-table-column prop="totals" label="面试题数量" width="120"></el-table-column>
        <el-table-column label="操作" align="center">
          <el-link>学科分类</el-link>
          <el-link>学科标签</el-link>
          <el-link>修改</el-link>
          <el-link>删除</el-link>
        </el-table-column>
      </el-table>
      <!-- 分页 -->
      <el-pagination
        background
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :current-page="page.currentPage"
        :page-sizes="[10, 15, 20, 25]"
        :page-size="page.pageSize"
        layout="prev, pager, next, sizes, jumper"
        :total="page.total"
      ></el-pagination>
    </el-card>
  </div>
</template>

<script>
import { list } from '@/api/hmmm/subjects'
export default {
  name: 'SubjectsList',
  data() {
    return {
      tableData: [],
      page: {
        currentPage: 1,
        total: 0,
        pageSize: 10
      },
      subjectName: ''
    }
  },
  methods: {
    // 前台是否显示
    orIsFrontDisplay(row, column, cellValue) {
      return cellValue === 1 ? '是' : '否'
    },

    // 页面显示条数改变事件
    handleSizeChange(newPage) {
      this.page.pageSize = newPage
      this.page.currentPage = 1
      this.getSubjectsList()
    },
    // 当前页改变事件
    handleCurrentChange(newPage) {
      this.page.currentPage = newPage
      this.getSubjectsList()
    },
    // 获取列表数据
    async getSubjectsList() {
      let data = {
        page: this.page.currentPage,
        pagesize: this.page.pageSize,
        subjectName: this.subjectName
      }
      let res = await list(data)
      this.tableData = res.data.items
      this.page.total = res.data.counts
    }
  },
  created() {
    this.getSubjectsList()
  }
}
</script>

<style  lang='less' scoped>
.dashboard-container {
  padding: 30px;
  .left {
    float: left;
    background-color: #ddd;
    font-size: 14px;
  }
  .right {
    float: right;
  }
  .el-row {
    margin-bottom: 20px;
  }
  .el-pagination {
    margin-top: 20px;
  }
  a {
    margin-right: 10px;
    color: rgb(16, 142, 233);
  }
}
</style>
