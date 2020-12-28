<template>
  <div class="container">
    <el-card>
      <!-- 头部 -->
      <div class="btn_wrapper">
        <span style="font-size: 12px; color: pink;">说明：目前支持学科和关键字条件筛选</span>
        <button
          type="button"
          class="el-button el-button--success el-button--small"
          @click="$router.push('/questions/new')"
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
                <label class="el-form-item__label" style="width: 80px;">学科</label>
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
                <label class="el-form-item__label" style="width: 80px;">二级目录</label>
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
                <label class="el-form-item__label" style="width: 80px;">标签</label>
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
                <label class="el-form-item__label" style="width: 80px;">关键字</label>
                <el-input placeholder="根据题干搜索" v-model="ListSelect.searchs"></el-input>
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
                <label class="el-form-item__label" style="width: 80px;">试题类型</label>
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
                <label class="el-form-item__label" style="width: 80px;">难度</label>
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
                <label class="el-form-item__label" style="width: 80px;">方向</label>
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
                <label class="el-form-item__label" style="width: 80px;">录入人</label>
                <el-select v-model="ListSelect.creators" placeholder="请选择">
                  <el-option
                    v-for="creatorItem in simpleName"
                    :key="creatorItem.id"
                    :label="creatorItem.username"
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
                <label class="el-form-item__label" style="width: 80px;">题目备注</label>
                <el-input class="custom_ipt" v-model="ListSelect.customipt1" />
              </el-form-item>
            </el-col>
            <el-col :span="6">
              <el-form-item prop="customipt2">
                <label class="el-form-item__label" style="width: 80px;">企业简称</label>
                <el-input class="custom_ipt" v-model="ListSelect.customipt2" />
              </el-form-item>
            </el-col>
            <el-col :span="6">
              <el-form-item prop="province">
                <label class="el-form-item__label" style="width: 80px;">城市</label>
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
                <el-select v-model="ListSelect.city" placeholder="请选择" style="width: 33%;">
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
              <div class="el-form-item__content relative_right" style="margin-left: 80px;">
                <button
                  type="button"
                  class="el-button btn1 el-button--default el-button--small"
                  @click="ClearAll"
                >
                  <span>清除</span></button
                ><button
                  type="button"
                  @click="getUserquerestlist()"
                  class="el-button btn2 el-button--primary el-button--small"
                >
                  <span>搜索</span>
                </button>
              </div>
            </el-col>
          </el-row>
        </div>
      </el-form>
      <!-- 中间分割警告字条 -->
      <el-alert type="info" show-icon :closable="false">
        <template slot="title"> 一共{{ this.total }}条数据 </template>
      </el-alert>
      <!-- 表格区域 -->
      <el-table :data="querestlist" stripe style="margin-top: 20px; ">
        <el-table-column label="试题编号" prop="number" width="120px"></el-table-column>
        <el-table-column label="学科" prop="subject"></el-table-column>
        <el-table-column label="目录" prop="catalog"></el-table-column>
        <el-table-column label="题型" v-model="questionType">
          <template slot-scope="scope">
            {{ questionType[scope.row.questionType].label }}
          </template>
        </el-table-column>
        <el-table-column label="题干" width="280px">
          <template slot-scope="scope">
            <p v-html="scope.row.question"></p>
          </template>
        </el-table-column>
        <el-table-column label="录入时间" width="180px">
          <template slot-scope="scope">
            {{ scope.row.addDate | parseTime }}
          </template>
        </el-table-column>
        <el-table-column label="难度">
          <template slot-scope="scope" v-model="difficulty">
            {{ difficulty[scope.row.difficulty].label }}
          </template>
        </el-table-column>
        <el-table-column label="录入人" prop="creator"></el-table-column>
        <el-table-column label="操作" width="180px" class="tab_btn">
          <template slot-scope="scope">
            <!-- // 查看 -->
            <el-tooltip class="item" effect="dark" content="预览" placement="top-start">
              <el-button
                type="primary"
                icon="el-icon-view"
                size="small"
                class="radius_btn"
                @click="questionItemClick(scope.row)"
              ></el-button>
            </el-tooltip>
            <!-- 修改 -->
            <el-tooltip class="item" effect="dark" content="修改" placement="top-start">
              <el-button
                type="success"
                icon="el-icon-edit"
                size="small"
                class="radius_btn btn_edit"
                @click="$router.push('/questions/new')"
              ></el-button>
            </el-tooltip>
            <!-- 删除 -->
            <el-tooltip class="item" effect="dark" content="删除" placement="top-start">
              <el-button
                type="danger"
                icon="el-icon-delete"
                size="small"
                class="radius_btn btn_delete"
                @click="deleteQuestions(scope.row.id)"
              ></el-button>
            </el-tooltip>
            <!-- 加入精选 -->
            <el-tooltip class="item" effect="dark" content="加入精选" placement="top-start">
              <el-button
                type="warning"
                icon="el-icon-check"
                size="small"
                class="radius_btn btn_check"
                @click="Addchoice(scope.row.id)"
              ></el-button>
            </el-tooltip>
          </template>
        </el-table-column>
      </el-table>
      <!-- 分页 -->

      <el-pagination
        style="padding-top:20px;"
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :current-page="queryInfo.pages"
        :page-sizes="[5, 10, 15, 20]"
        :page-size="queryInfo.pagesize"
        layout="->,total, sizes, prev, pager, next, jumper"
        :total="total"
      >
      </el-pagination>
    </el-card>
    <questions-preview
      :previewDialogVisible="previewDialogVisible"
      :querestItem="querestItem"
      v-model="querestlist"
      @close="previewDialogVisible = false"
    />
  </div>
</template>

<script>
import { querestlist, detail, randoms, remove, choiceAdd } from '@/api/hmmm/questions'
import { list } from '@/api/hmmm/subjects'
import { Directorylist } from '@/api/hmmm/directorys'
import { Taglist } from '@/api/hmmm/tags'
import { questionType, direction, difficulty } from '@/api/hmmm/constants'
import { provinces, citys } from '@/api/hmmm/citys'
import QuestionsPreview from '../components/questions-preview'
import { simple } from '@/api/base/users'

export default {
  components: {
    QuestionsPreview
  },
  data() {
    return {
      querestItem: {},
      previewDialogVisible: false,
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
      simpleName: [],
      twoDirectorlist: [],
      threeDirectorlist: [],
      list: [],
      ListSubject: [],
      querestlist: [],
      cityList: [],
      questionType: questionType,
      direction: direction,
      difficulty: difficulty,
      provincesList: provinces,
      // 查询对象
      queryInfo: {
        pages: 1,
        pagesize: 10
      },
      // 总数据条数
      total: 0
    }
  },
  created() {
    this.getUserlist()
    this.getUserquerestlist()
    this.questinsSimple()
  },
  mounted() {
    // this.getUserlist()
  },
  methods: {
    async getUserlist() {
      const { data: res } = await list()
      try {
        this.list = res.items
        // console.log(res.items)
        let arr = []
        res.items.forEach((item, index) => {
          arr[index] = {
            subjcetName: item.subjectName
          }
        })
        this.ListSubject = arr
        // console.log(this.ListSubject)
        // console.log('获取成功')
        this.getUserSubject()
        this.getUserTags()
      } catch (error) {
        this.$message.error('获取学科名称错误！')
      }
    },
    async getUserquerestlist() {
      const { data: res } = await querestlist(this.queryInfo)

      try {
        this.querestlist = res.items
        // console.log(res)
        // this.querestlist.push(this.questionType,this.difficulty)
        // console.log(this.questionType.label)
        // if (this.querestlist.questionType === this.questionType.value)
        // {
        //   this.querestlist.questionType = this.questionType.label
        // }
        // this.querestlist = this.querestlist.flat(Infinity)
        // console.log(this.querestlist)
        this.total = res.counts

        // console.log(res.items)
      } catch (err) {
        this.$message.success('获取了错误的数据值')
      }
    },
    // 查询页码发请求

    handleSizeChange(newSize) {
      this.queryInfo.pagesize = newSize
      this.getUserlist()
      this.getUserquerestlist()
    },
    handleCurrentChange(newPage) {
      this.queryInfo.page = newPage
      this.getUserlist()
      this.getUserquerestlist()
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
      // console.log(this.$refs.questionsRefs)
      this.$refs.questionsRefs.resetFields()
    },
    async questionItemClick(val) {
      const { data } = await detail({
        id: val.id
      })
      // console.log(data)
      this.querestItem = data
      this.previewDialogVisible = true
      // console.log(data)
    },
    async questinsSimple() {
      const { data: res } = await simple()
      // console.log(res)
      this.simpleName = res
    },
    // 删除数据
    async deleteQuestions(id) {
      const { data: res } = await remove({
        id
      })
      this.$message.success('删除成功')
      this.getUserquerestlist()
    },
    async Addchoice(id) {
      const confirmResult = await this.$confirm('此操作将该题目加入精选, 是否继续?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'info'
      }).catch(err => err)
      if (confirmResult !== 'confirm') {
        const { data: res } = await choiceAdd({
          id,
          choiceState: 0
        })
        return this.$message.info('已经取消精选！')
      }

      this.$message.success('加入精选成功！')
      this.getUserquerestlist()
    },
    searchClick() {}
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
.cell {
  text-align: center;
  .radius_btn {
    color: #409eff;
    background: #ecf5ff;
    border-color: #b3d8ff;
    padding: 9px;
    width: 32px;
    height: 32px;
    border-radius: 50%;
  }
  .btn_edit {
    color: #67c23a;
    background: #f0f9eb;
    border-color: #c2e7b0;
  }
  .btn_delete {
    color: #f56c6c;
    background: #fef0f0;
    border-color: #fbc4c4;
  }
  .btn_check {
    color: #e6a23c;
    background: #fdf6ec;
    border-color: #f5dab1;
  }
}
</style>
