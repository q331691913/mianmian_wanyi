<template>
  <div class="container">
    <el-card>
      <!-- 头部 -->
      <div class="btn_wrapper">
        <span style="font-size: 12px; color: pink;"
          >说明：目前支持学科和关键字条件筛选</span
        >
        <button
          type="button"
          class="el-button el-button--success el-button--small"
        >
          <i class="el-icon-edit"></i>
          <span>新增试题</span>
        </button>
      </div>
      <!-- 下拉选择框模块 -->

      <el-form ref="questionsRefs" :model="ListSelect">
        <div class="w-container">
          <!-- 第一行 -->
          <el-row>
            <el-col :span="6">
              <el-form-item prop="tesID">
                <label class="el-form-item__label" style="width: 80px;"
                  >学科</label
                >
                <el-select v-model="ListSelect.tesID" placeholder="请选择">
                  <el-option
                    v-for="(item, index) in list"
                    :key="item.id"
                    :label="item.subjectName"
                    :value="item.id"
                    @change="selectExitSelectConfig"
                    filterable
                    remote
                  ></el-option>
                </el-select>
              </el-form-item>
            </el-col>
            <el-col :span="6">
              <el-form-item prop="numIids">
                <label class="el-form-item__label" style="width: 80px;"
                  >二级目录</label
                >
                <el-select v-model="ListSelect.numIids" placeholder="请选择">
                  <el-option
                    v-for="twoitem in twoDirectorlist"
                    :value="twoitem.id"
                    ::key="twoitem.id"
                    :label="twoitem.directoryName"
                  ></el-option>
                </el-select>
              </el-form-item>
            </el-col>

            <el-col :span="6">
              <el-form-item prop="threeList">
                <label class="el-form-item__label" style="width: 80px;"
                  >标签</label
                >
                <el-select v-model="ListSelect.threeList" placeholder="请选择">
                  <el-option
                    v-for="treeItem in threeDirectorlist"
                    :label="treeItem.tagName"
                    :value="treeItem.id"
                    :key="treeItem.id"
                  ></el-option>
                </el-select>
              </el-form-item>
            </el-col>
            <el-col :span="4" class="guanjian">
              <el-form-item class="el-iptRight" prop="searchs">
                <label class="el-form-item__label" style="width: 80px;"
                  >关键字</label
                >
                <el-input
                  placeholder="根据题干搜索"
                  v-model="ListSelect.searchs"
                ></el-input>
                <!-- <el-select placeholder="请选择">
                  <el-option value="1"></el-option>
                </el-select> -->
              </el-form-item>
            </el-col>
          </el-row>
          <!-- 第二行 -->
          <el-row>
            <el-col :span="6">
              <el-form-item prop="typeQuest">
                <label class="el-form-item__label" style="width: 80px;"
                  >试题类型</label
                >
                <el-select v-model="ListSelect.typeQuest" placeholder="请选择">
                  <el-option
                    v-for="typeItem in this.questionType"
                    :key="typeItem.value"
                    :label="typeItem.label"
                    :value="typeItem.vlue"
                  ></el-option>
                </el-select>
              </el-form-item>
            </el-col>
            <el-col :span="6">
              <el-form-item prop="defiy">
                <label class="el-form-item__label" style="width: 80px;"
                  >难度</label
                >
                <el-select v-model="ListSelect.defiy" placeholder="请选择">
                  <el-option
                    v-for="difficultyItem in difficulty"
                    :key="difficultyItem.value"
                    :label="difficultyItem.label"
                    :value="difficultyItem.value"
                  ></el-option>
                </el-select>
              </el-form-item>
            </el-col>

            <el-col :span="6">
              <el-form-item prop="direct">
                <label class="el-form-item__label" style="width: 80px;"
                  >方向</label
                >
                <el-select v-model="ListSelect.direct" placeholder="请选择">
                  <el-option
                    v-for="(directionItem, index) in direction"
                    :key="index"
                    :label="directionItem"
                    :value="index"
                  ></el-option>
                </el-select>
              </el-form-item>
            </el-col>

            <el-col :span="6">
              <el-form-item prop="creators">
                <label class="el-form-item__label" style="width: 80px;"
                  >录入人</label
                >
                <el-select v-model="ListSelect.creators" placeholder="请选择">
                  <el-option
                    v-for="creatorItem in querestlist"
                    :key="creatorItem.id"
                    :label="creatorItem.creator"
                    :value="creatorItem.id"
                  ></el-option>
                </el-select>
              </el-form-item>
            </el-col>
          </el-row>
          <!-- 第三行 -->
          <el-row>
            <el-col :span="6">
              <el-form-item prop="customipt1">
                <label class="el-form-item__label" style="width: 80px;"
                  >题目备注</label
                >
                <el-input class="custom_ipt" v-model="ListSelect.customipt1" />
              </el-form-item>
            </el-col>
            <el-col :span="6">
              <el-form-item prop="customipt2">
                <label class="el-form-item__label" style="width: 80px;"
                  >企业简称</label
                >
                <el-input class="custom_ipt" v-model="ListSelect.customipt2" />
              </el-form-item>
            </el-col>
            <el-col :span="6">
              <el-form-item prop="province">
                <label class="el-form-item__label" style="width: 80px;"
                  >城市</label
                >
                <el-select
                  v-model="ListSelect.province"
                  placeholder="请选择"
                  style="width: 32%; margin-right: 3%;"
                  @change="CityChange"
                >
                  <el-option
                    v-for="(item, index) in this.provincesList"
                    :key="index"
                    :label="item"
                    :value="item"
                  ></el-option>
                </el-select>
                <el-select
                  v-model="ListSelect.city"
                  placeholder="请选择"
                  style="width: 33%;"
                >
                  <el-option
                    v-for="(item, index) in cityList"
                    :key="index"
                    :label="item"
                    :value="item"
                  ></el-option>
                </el-select>
              </el-form-item>
            </el-col>
            <el-col :span="6">
              <div
                class="el-form-item__content relative_right"
                style="margin-left: 80px;"
              >
                <button
                  type="button"
                  class="el-button btn1 el-button--default el-button--small"
                  @click="ClearAll"
                >
                  <span>清除</span></button
                ><button
                  type="button"
                  class="el-button btn2 el-button--primary el-button--small"
                >
                  <span>搜索</span>
                </button>
              </div>
            </el-col>
          </el-row>
        </div>
      </el-form>
    </el-card>
  </div>
</template>

<script>
import { querestlist, detail, randoms } from '@/api/hmmm/questions'
import { list } from '@/api/hmmm/subjects'
import { Directorylist } from '@/api/hmmm/directorys'
import { Taglist } from '@/api/hmmm/tags'
import { questionType, direction, difficulty } from '@/api/hmmm/constants'
import { provinces, citys } from '@/api/hmmm/citys'
export default {
  data() {
    return {
      ListSelect: {
        tesID: null,
        numIids: [],
        threeList: [],
        defiy: '',
        direct: '',
        creators: '',
        typeQuest: '',
        customipt1: '',
        customipt2: '',
        province: '',
        city: '',
        searchs: ''
      },
      twoDirectorlist: [],
      threeDirectorlist: [],
      list: [],
      querestlist: [],
      cityList: [],
      questionType: questionType,
      direction: direction,
      difficulty: difficulty,
      provincesList: provinces
    }
  },
  created() {
    this.getUserlist()
    this.getUserquerestlist()
    // this.getUserRandomslList()
  },
  mounted() {
    // this.getUserlist()
  },
  methods: {
    async getUserlist() {
      const { data: res } = await list()
      try {
        this.list = res.items
        // console.log('获取成功')
        // console.log(res)
        this.getUserSubject()
        this.getUserTags()
      } catch (error) {
        this.$message.error('获取学科名称错误！')
      }
    },
    async getUserquerestlist() {
      const { data: res } = await querestlist()
      try {
        this.querestlist = res.items
      } catch (err) {
        console.log('错误')
      }
    },
    selectExitSelectConfig() {
      this.twoDirectorlist = []
      this.ListSelect.numIids = null
      this.ListSelect.threeList = null
      this.getUserSubject()
      this.getUserTags()
    },
    async getUserSubject() {
      const { data: res } = await Directorylist()
      this.twoDirectorlist = res.items
    },
    async getUserTags() {
      const { data: res } = await Taglist()
      this.threeDirectorlist = res.items
    },
    // 当点击的城市等于后台的城市时显示他的城区
    async CityChange() {
      this.ListSelect.city = ''
      let arr = []
      arr = citys(this.ListSelect.province)
      // console.log(arr)
      this.cityList = arr
    },
    // 清空所有表单数据
    ClearAll() {
      console.log(this.$refs.questionsRefs)
      this.$refs.questionsRefs.resetFields()
    }
  },
  watch: {
    list: {
      handler(newValue, oldValue) {},
      deep: true
    },
    twoDirectorlist: {
      handler(newValue, oldValue) {},
      deep: true
    }
  }
}
</script>

<style scoped lang="less">
.btn_wrapper {
  margin-bottom: 15px;
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-pack: justify;
  -ms-flex-pack: justify;
  justify-content: space-between;
}
.el-form-item__label {
  width: 80px;
  text-align: right;
  vertical-align: middle;
  float: left;
  font-size: 14px;
  color: #606266;
  line-height: 40px;
  padding: 0 12px 0 0;
}
.w-container {
  white-space: nowrap !important;
}
.el-iptRight {
  padding: 0px;
  box-sizing: content-box;
  .el-input__inner {
    padding: 0 !important;
  }
}
.custom_ipt {
  width: 68%;
}
.relative_right {
  position: relative;
  .btn2 {
    position: absolute;
    top: 0;
    right: 0px;
  }
  .btn1 {
    position: absolute;
    top: 0;
    right: 63px;
  }
}
</style>
