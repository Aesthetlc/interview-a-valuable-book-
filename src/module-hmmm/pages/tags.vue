<template>
  <div class="dashboard-container">
    <div class="app-container">
      <el-row>
        <el-button>新建标签</el-button>
        <el-button>返回学科</el-button>
      </el-row>
      <el-row>
        标签名称：
        <el-input style="width:300px" v-model="tagName" />
      </el-row>
      <el-table :data="tableData" border style="width: 100%">
        <el-table-column prop="id" label="序号" width="100" align="center"></el-table-column>
        <el-table-column prop="tagName" label="标签名称" width="200"></el-table-column>
        <el-table-column prop="creatorID" label="创建者" width="200" align="center">
        </el-table-column>
        <el-table-column prop="addDate" label="创建日期" width="250"></el-table-column>
        <el-table-column prop="totals" label="面试题数量" width="150"></el-table-column>
        <el-table-column prop="state" label="状态" width="150" align="center">
          <template slot-scope="scope">
            <span>{{ getStatus(scope.row.state)}}</span>
          </template>
        </el-table-column>
        <el-table-column label="操作">
          <el-button type="primary">修改</el-button>
          <el-button type="success">启用</el-button>
          <el-button type="danger">禁用</el-button>
          <el-button type="warning">删除</el-button>
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
    </div>
  </div>
</template>

<script>
// 标签列表
import { list } from '@/api/hmmm/tags'
// 状态
import { status } from '@/api/hmmm/constants'
export default {
  name: 'TagsList',
  data() {
    return {
      tableData: [],
      page: {
        currentPage: 1,
        total: 0,
        pageSize: 10
      },
      tagName: ''
    }
  },
  methods: {
    // 获取状态
    getStatus(stat) {
      return status.filter((item, index) => {
        if (item.value === stat) {
          return item.label
        }
      })[0].label
    },

    // 页面显示条数改变事件
    handleSizeChange(newPage) {
      this.page.pageSize = newPage
      this.page.currentPage = 1
      this.getTagsList()
    },
    // 当前页改变事件
    handleCurrentChange(newPage) {
      this.page.currentPage = newPage
      this.getTagsList()
    },
    // 获取标签列表
    async getTagsList() {
      let data = {
        page: this.page.currentPage,
        pagesize: this.page.pageSize,
        tagName: this.tagName
      }
      let res = await list(data)
      this.tableData = res.data.items
      this.page.total = res.data.counts
    }
  },
  created() {
    this.getTagsList()
  }
}
</script>

<style scoped>
</style>
