<template>
  <div class="dashboard-container">
    <div class="add-form">
      <el-dialog :title="titleInfo.text+'面试技巧'" :visible.sync="dialogFormVisible">
        <el-form :rules="rules" ref="articlesFrom" :model="formBase.data" label-width="80px">
          <el-form-item label="标题" prop="title">
            <el-input style="width:100%" v-model="formBase.data.title"></el-input>
          </el-form-item>

          <el-form-item label="正文" prop="articleBody">
            <el-input
              style="width:100%"
              type="textarea"
              :rows="5"
              placeholder="请输入内容"
              v-model="formBase.data.articleBody"
            ></el-input>
          </el-form-item>

          <el-form-item label="视频地址" prop="videoURL">
            <el-input style="width:100%" v-model="formBase.data.videoURL"></el-input>
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
import { add, update } from '@/api/hmmm/articles'
export default {
  name: 'ArticlesAdd',
  props: {
    titleInfo: {
      type: Object,
      required: true
    },
    formBase: {
      type: Object,
      required: true
    }
    // aaa: {
    //    type: Object,
    //   required: true
    // }
  },
  data() {
    return {
      dialogFormVisible: false,
      rules: {
        title: [{ required: true, message: '标题不能为空' }],
        articleBody: [{ required: true, message: '正文不能为空' }]
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
          if (this.formBase.data.id) {
            await update(this.formBase.data)
            this.$message({ message: '修改成功', type: 'success' })
          } else {
            await add(this.formBase.data)
            this.$message({ message: '添加成功', type: 'success' })
          }
          this.$emit('shuaxin')
          this.dialogFormVisible = false
         
        }
      })
    },
    // 取消添加
    closeArticles() {
      this.dialogFormVisible = false
    }
  },
  created() {
    
  }
}
</script>

<style scoped>
</style>
