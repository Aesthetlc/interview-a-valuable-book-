<template>
  <div class="dashboard-container">
    <div class="add-form">
      <el-dialog title="新增面试技巧" :visible.sync="dialogFormVisible">
        <el-form :rules="rules" ref="articlesFrom" :model="articlesList" label-width="100px">
          <el-form-item label="标题" prop="title">
            <el-input style="width:800px" v-model="articlesList.title"></el-input>
          </el-form-item>

          <el-form-item label="正文" prop="articleBody">
            <el-input
              style="width:800px"
              type="textarea"
              :rows="5"
              placeholder="请输入内容"
              v-model="articlesList.articleBody"
            ></el-input>
          </el-form-item>

          <el-form-item label="视频地址" prop="videoURL">
            <el-input style="width:800px" v-model="articlesList.videoURL"></el-input>
          </el-form-item>

          <el-row type="flex" justify="center">
            <el-button type="primary" style="width:300px" @click="addArticles">新增</el-button>
            <el-button style="width:300px" @click="closeArticles">取消</el-button>
          </el-row>
        </el-form>
      </el-dialog>
    </div>
  </div>
</template>

<script>
import { add } from '@/api/hmmm/articles'
export default {
  name: 'ArticlesAdd',
  data() {
    return {
      dialogFormVisible: false,
      articlesList: {}, // 数据form
      rules: {
        title: [{ required: true, message: '标题不能为空' }],
        articleBody: [{ required: true, message: '标题不能为空' }]
      }
    }
  },
  methods: {
    // 弹层显示
    dialogFormV() {
      this.dialogFormVisible = true
    },
    // 添加面试技巧
    addArticles() {
      this.$refs.articlesFrom.validate(async isOk => {
        if (isOk) {
          await add(this.articlesList)
          this.$message({ message: '添加成功', type: 'success' })
          this.dialogFormVisible = false
          this.$router.push('/articles/list')
        }
      })
    },
    // 取消添加
    closeArticles() {
      this.dialogFormVisible = false
    }
  }
}
</script>

<style scoped>
</style>
