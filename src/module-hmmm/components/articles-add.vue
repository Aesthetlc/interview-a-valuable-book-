<template>
  <div class="dashboard-container">
    <div class="add-form">
      <el-dialog title="新增面试技巧" :visible.sync="dialogFormVisible">
        <el-form ref="articlesFrom" :model="articlesFrom">
          <el-form-item label="标题" prop="title">
            <el-input style="width:800px" v-model="articlesFrom.title"></el-input>
          </el-form-item>

          <el-form-item label="正文" prop="正文"></el-form-item>
        </el-form>
      </el-dialog>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ArticlesAdd',
  data() {
    return {
      dialogFormVisible: false,
      articlesFrom: {}
    }
  },
  methods: {
    // 弹层显示
    dialogFormV() {
      this.dialogFormVisible = true
    }
  }
}
</script>

<style scoped>
</style>
