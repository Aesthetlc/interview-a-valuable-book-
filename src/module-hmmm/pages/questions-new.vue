<template>
  <div class="dashboard-container">
    <div class="app-container">
      <el-form>
        <el-form-item label="学科">
          <el-select v-model="formData.subjectID">
            <el-option
              v-for="item in subjectsList"
              :key="item.value"
              :label="item.label"
              :value="item.value"
            ></el-option>
          </el-select>
        </el-form-item>

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

         <el-form-item label="企业：">
          <el-select v-model="formData.enterpriseID">
            <el-option v-for="item in enterpriseIDList" :key="item.id" :value="item.id" :label="item.company"></el-option>
          </el-select>
        </el-form-item>

        <el-form-item label="城市">
          <el-select
            style="width:120px"
            placeholder="请选择"
            @change="getCitysList"
            v-model="formData.province"
          >
            <el-option v-for="item in provinces()" :key="item" :label="item" :value="item"></el-option>
          </el-select>
          <el-select placeholder="请选择" style="width:120px" v-model="formData.citys">
            <el-option v-for="item in citysList" :key="item" :label="item" :value="item"></el-option>
          </el-select>
        </el-form-item>

        <el-form-item label="方向">
          <el-select style="width:245px" v-model="formData.direction">
            <el-option v-for="item in directionList" :key="item" :label="item" :value="item"></el-option>
          </el-select>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
import { simple } from '@/api/hmmm/subjects'
import { provinces, citys } from '@/api/hmmm/citys'
import { simple as directorysSimple } from '@/api/hmmm/directorys'
import { direction as directionList } from '@/api/hmmm/constants'
import { list as companysList } from '@/api/hmmm/companys'
export default {
  name: 'QuestionsNew',
  data() {
    return {
      subjectsList: [], // 学科
      catalogIDList: [], // 二级目录
      citysList: [], // 区域
      directionList, // 方向
      enterpriseIDList: [], // 企业
      formData: {
        subjectID: '',
        catalogID: '',
        province: '', // 城市
        city: '', // 区域
        direction: '', // 方向
        enterpriseID: ''// 企业
      }
    }
  },
  methods: {
    // 获取企业的方法
     async getCompanyList() {
      let result = await companysList()
      this.enterpriseIDList = result.data.items
     
    },
    // 获取学科的方法
    async getSubjectsList() {
      let result = await simple()
      this.subjectsList = result.data
    },
    // 获取城市的方法
    provinces,
    // 获取区域的方法
    async getCitysList(name) {
      this.citysList = await citys(name)
    },
    // 获取二级目录的方法
    async getCatalogIDList() {
      let result = await directorysSimple()
      this.catalogIDList = result.data
    }
  },
  created() {
    this.getSubjectsList()
    this.getCatalogIDList()
    this.getCompanyList()
  }
}
</script>

<style scoped>
</style>
