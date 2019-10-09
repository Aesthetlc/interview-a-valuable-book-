<template>
  <div class="dashboard-container">
    <el-card style="margin:20px">
      <el-button @click="handleChange('add')">新增面试技巧</el-button>
      <div class="search">
        <div class="left">
          <div class="searchWords">
            <span>关键词</span>
          </div>
          <div class="searchInput">
            <el-input
              @change="getSearchMsg(searchWords)"
              v-model="searchWords"
              placeholder="请输入题目编号/题干"
            ></el-input>
          </div>
        </div>
        <div class="right">
          <el-button @click="clearKeywords">清除</el-button>
          <el-button @click="getSearchMsg(searchWords)" type="primary">搜索</el-button>
        </div>
      </div>
      <el-table
        v-loading="listLoading"
        :data="ArticlesList"
        border
        style="width: 100%"
        class="table"
      >
        <el-table-column label="序号" align="center" type="index"></el-table-column>
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
        <el-table-column label="状态" align="center" prop="state" :formatter="statusFMT"></el-table-column>
        <el-table-column align="center" label="录入人">
          <template slot-scope="scope">
            <span>{{scope.row.creator}}</span>
          </template>
        </el-table-column>
        <el-table-column label="操作">
          <template slot-scope="obj">
            <el-link type="primary">预览</el-link>
            <el-link type="primary" @click="handleChange(obj.row)">修改</el-link>
            <el-link type="primary" @click="delArticlesMsg(obj.row)">删除</el-link>
            <el-link
              type="primary"
              @click="forbiddenArticlesMsg(obj.row)"
            >{{obj.row.state?"禁用":"启用"}}</el-link>
          </template>
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
              :page-sizes="[3,6,9,10]"
              layout="sizes, prev, pager, next, jumper"
            ></el-pagination>
          </div>
        </div>
        <!-- end -->
      </el-row>

      <!-- 新增标签弹层 -->
      <Dialog ref="editUser" :titleInfo="titleInfo" :formBase="formData"></Dialog>
    </el-card>
  </div>
</template>

<script>
import { list, remove, state, detail } from '@/api/hmmm/articles'
import { status } from '@/api/hmmm/constants' // 常量数据
import Dialog from './../components/articles'
export default {
  name: 'ArticlesList',
  components: {
    Dialog
  },
  data() {
    return {
      ArticlesList: [], // 获取面试技巧数据
      listLoading: true,
      page: {
        currentPage: 1,
        total: 0,
        pageSize: 6
      },
      searchWords: '', // 搜索关键词
      titleInfo: {
        text: '' // 新增、编辑
      },
      formData: {
        data: {
          title: '',
          articleBody: '',
          creatorID: '',
          state: '',
          videoURL: '',
          visits: ''
        }
      }
    }
  },
  watch: {
    searchWords(newValue) {
      this.searchWords = newValue
      this.getArticlesSkill()
    }
  },
  methods: {
    // 新增面试信息
    handleChange(val) {
      this.$refs.editUser.dialogFormV()
      if (val === 'add') {
        this.titleInfo.text = '新增'
      } else {
        this.titleInfo.text = '编辑'
        this.hanldeEditForm(val)
      }
    },
    // 获取编辑的信息 传递给弹窗
    async hanldeEditForm(val) {
      const res = await detail(val)
      // 获取详情 给formData
      this.formData = res
    },
    // 禁用状态
    forbiddenArticlesMsg(obj) {
      let condition = obj.state ? '禁用' : '启用'
      this.$confirm(`您要${condition}此条信息么？`, '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        if (Number(obj.state) === 1) {
          obj.state = 0
        } else {
          obj.state = 1
        }
        state(obj)
        this.$message({
          type: 'success',
          message: `${condition}成功!`
        })
        this.getArticlesSkill()
      })
    },
    // 状态过滤
    statusFMT(row, column, cellValue) {
      let result = status.filter(item => {
        if (item.value === cellValue) {
          return item.label
        }
      })
      return result[0]['label']
    },
    // 删除面试信息
    delArticlesMsg(obj) {
      this.$confirm('您要删除此条信息么？', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        state(obj)
        this.$message({
          type: 'success',
          message: '删除成功!'
        })
        this.getArticlesSkill()
      })
    },
    async getArticlesSkill() {
      let data = {
        page: this.page.currentPage,
        pagesize: this.page.pageSize,
        keyword: this.searchWords
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
    },

    // 搜索关键词
    getSearchMsg(keyword) {
      this.searchWords = keyword
      this.getArticlesSkill()
    },

    // 重置
    clearKeywords() {
      this.searchWords = ''
      this.getArticlesSkill()
    }
  },
  created() {
    this.getArticlesSkill()
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
