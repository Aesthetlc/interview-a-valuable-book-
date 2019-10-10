<template>
  <div class="dashboard-container">
    <el-card style="margin:20px">
      <el-button>新增试题</el-button>
      <el-button>批量导入</el-button>
      <el-form>
        <el-row>
          <el-col :span="6">
            <el-form-item label="学科">
              <el-select placeholder="请选择" v-model="formData.subjectID">
                <el-option
                  v-for="item in subjectList"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value"
                ></el-option>
              </el-select>
            </el-form-item>
          </el-col>
          <el-col :span="6">
            <el-form-item label="难度">
              <el-select placeholder="请选择" v-model="formData.difficulty">
                <el-option
                  v-for="item in difficultyList"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value"
                ></el-option>
              </el-select>
            </el-form-item>
          </el-col>
          <el-col :span="6">
            <el-form-item label="试题类型">
              <el-select placeholder="请选择" v-model="formData.questionType">
                <el-option
                  v-for="item in questionTypeList"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value"
                ></el-option>
              </el-select>
            </el-form-item>
          </el-col>
          <el-col :span="6">
            <el-form-item label="标签">
              <el-select placeholder="请选择" v-model="formData.tags">
                <el-option
                  v-for="item in tagsList"
                  :key="item.value"
                  :value="item.value"
                  :label="item.label"
                ></el-option>
              </el-select>
            </el-form-item>
          </el-col>
        </el-row>
        <!-- ************************************************** -->
        <el-row>
          <el-col :span="6">
            <el-form-item label="城市">
              <el-select
                style="width:120px"
                placeholder="请选择"
                @change="getCitysList(formData.province)"
                v-model="formData.province"
              >
                <el-option v-for="item in provinces()" :key="item" :label="item" :value="item"></el-option>
              </el-select>
              <el-select placeholder="请选择" style="width:120px" v-model="formData.citys">
                <el-option v-for="item in citysList" :key="item" :label="item" :value="item"></el-option>
              </el-select>
            </el-form-item>
          </el-col>

          <el-col :span="6">
            <el-form-item label="关键词">
              <el-input style="width:250px" v-model="formData.keyword"></el-input>
            </el-form-item>
          </el-col>

          <el-col :span="6">
            <el-form-item label="题目备注">
              <el-input style="width:250px" v-model="formData.remarks"></el-input>
            </el-form-item>
          </el-col>

          <el-col :span="6">
            <el-form-item label="企业简称">
              <el-input style="width:250px" v-model="formData.shortName"></el-input>
            </el-form-item>
          </el-col>
        </el-row>

        <el-row>
          <el-col :span="6">
            <el-form-item label="方向">
              <el-select v-model="formData.direction">
                <el-option v-for="item in directionList" :key="item" :label="item" :value="item"></el-option>
              </el-select>
            </el-form-item>
          </el-col>
          <el-col :span="6">
            <el-form-item label="录入人">
              <el-select v-model="formData.creatorID">
                <el-option
                  v-for="item in creatorIDList"
                  :key="item.id"
                  :label="item.username"
                  :value="item.id"
                ></el-option>
              </el-select>
            </el-form-item>
          </el-col>
          <el-col :span="6">
            <el-form-item label="二级目录">
              <el-select v-model="formData.catalogID">
                <el-option
                  v-for="item in catalogIDList"
                  :key="item.id"
                  :label="item.label"
                  :value="item.value"
                ></el-option>
              </el-select>
            </el-form-item>
          </el-col>
          <el-col :span="6">
            <el-button type="primary">搜索</el-button>
            <el-button>重置</el-button>
          </el-col>
        </el-row>
      </el-form>

      <el-table :data="questionList" >
        <el-table-column label="序号" type="index"></el-table-column>
        <el-table-column label="试题编号" prop="number"></el-table-column>
        <el-table-column label="学科" prop="subject"></el-table-column>
        <el-table-column label="题型" prop="questionType"></el-table-column>
        <el-table-column label="题干" prop="question"></el-table-column>
        <el-table-column label="录入时间"  prop="addDate"></el-table-column>
        <el-table-column label="难度" prop="difficulty"></el-table-column>
        <el-table-column label="录入人" prop="creator"></el-table-column>
        <el-table-column label="操作">
          <el-link>预览</el-link>
          <el-link>修改</el-link>
          <el-link>删除</el-link>
          <el-link>加入精选</el-link>
        </el-table-column>
      </el-table>
    </el-card>
  </div>
</template>

<script>
import { list } from '@/api/hmmm/questions'
import { simple } from '@/api/hmmm/subjects'
import { simple as tagsSimple } from '@/api/hmmm/tags'
import { provinces, citys } from '@/api/hmmm/citys'
import { simple as directorysSimple } from '@/api/hmmm/directorys'
import { simple as userSimple } from '@/api/base/users'
import {
  difficulty as difficultyList,
  questionType as questionTypeList,
  direction as directionList
} from '@/api/hmmm/constants'
export default {
  name: 'QuestionsList',
  data() {
    return {
      subjectList: [], // 学科
      difficultyList, // 难度
      questionTypeList, // 试题类型
      tagsList: [], // 标签
      citysList: [], // 区域
      directionList, // 方向
      creatorIDList: [], // 录入人
      catalogIDList: [], // 二级目录
      questionList: [], // 初始化数据
      formData: {
        subjectID: '', // 学科
        difficulty: '', // 难度
        questionType: '', // 试题类型
        tags: '', // 标签
        province: '', // 城市
        city: '', // 区域
        keyword: '', // 关键字
        remarks: '', // 题目备注
        shortName: '', // 企业简称
        direction: '', // 方向
        creatorID: '', // 录入人
        catalogID: '' // 目录
      }
    }
  },
  methods: {
    // 获取学科的方法
    async getSubjectList() {
      let result = await simple()
      this.subjectList = result.data
    },
    // 获取标签的方法
    async getTagsList() {
      let result = await tagsSimple()
      this.tagsList = result.data
    },
    // 获取城市的方法
    provinces,
    // 获取区域的方法
    getCitysList(name) {
      this.citysList = citys(name)
    },
    // 获取录入人的方法
    async getCreatorIDList() {
      let result = await userSimple()
      this.creatorIDList = result.data
    },
    // 获取二级目录的方法
    async getCatalogIDList() {
      let result = await directorysSimple()
      this.catalogIDList = result.data
    },
    // 获取数据 供table显示
    async getQuestionList() {
      let result = await list()
      this.questionList = result.data.items
    }
  },
  created() {
    this.getSubjectList()
    this.getTagsList()
    this.getCreatorIDList()
    this.getCatalogIDList()
    this.getQuestionList()
  }
}
</script>

<style scoped>
.el-row {
  margin-top: 10px;
}
</style>
