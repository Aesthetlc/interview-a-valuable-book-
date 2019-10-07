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

      <el-table
        v-loading="listLoading"
        :data="ArticlesList"
        border
        style="width: 100%"
        class="table"
      >
        <el-table-column align="center" label="序号">
          <template slot-scope="scope">
            <span>{{scope.row.id}}</span>
          </template>
        </el-table-column>
        <el-table-column align="center" label="标题">
          <template slot-scope="scope">
            <span>{{scope.row.title}}</span>
          </template>
        </el-table-column>
        <el-table-column align="center" label="阅读数">
          <template slot-scope="scope">
            <span>{{scope.row.reads}}</span>
          </template>
        </el-table-column>
        <el-table-column align="center" label="状态">
          <template slot-scope="scope">
            <span>{{scope.row.state | changeStatus}}</span>
          </template>
        </el-table-column>
        <el-table-column align="center" label="录入人">
          <template slot-scope="scope">
            <span>{{scope.row.creator}}</span>
          </template>
        </el-table-column>
        <el-table-column label="操作">
          <el-link type="primary">预览</el-link>
          <el-link type="primary">修改</el-link>
          <el-link type="primary">删除</el-link>
          <el-link type="primary">禁用</el-link>
        </el-table-column>
      </el-table>
      <el-row type="flex" align="center">
        <!-- 分页 -->
        <div class="pagination">
          <div class="pages">
            <el-pagination
              background
              @size-change="handleSizeChange"
              @current-change="handleCurrentChange"
              :current-page="page.currentPage"
              :total="page.total"
              :page-size="page.pageSize"
              :page-sizes="[1,2,3,4]"
              layout="sizes, prev, pager, next, jumper"
            ></el-pagination>
          </div>
        </div>
        <!-- end -->
      </el-row>
    </el-card>
  </div>
</template>

<script>
import { list } from '@/api/hmmm/articles'
export default {
  name: 'ArticlesList',
  data() {
    return {
      ArticlesList: [], // 获取面试技巧数据
      listLoading: true,
      page: {
       currentPage: 1,
       total: 0,
       pageSize: 1 
      }
    }
  },
  methods: {
    async getArticlesSkill() {
      let data = {
        page: this.page.currentPage,
        pagesize: this.page.pageSize
      }
      this.listLoading = true
      let result = await list(data)
      this.ArticlesList = result.data.items
      // 获取总条数
      this.page.total = result.data.counts
      // 加载关闭
      this.listLoading = false
    },

    // 每页显示信息条数
    handleSizeChange(newPage) {
      this.page.pageSize = newPage
      this.getArticlesSkill()
    },

    // 进入某一页
    handleCurrentChange(newPage) {
      this.page.currentPage = newPage
      this.getArticlesSkill()
    }
  },
  created() {
    this.getArticlesSkill()
  },
  filters: {
    changeStatus(status) {
      switch (status) {
        case 1:
          return '启用'
        case 2:
          return '禁用'
      }
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
.table {
  margin-top: 20px;
}
</style>
