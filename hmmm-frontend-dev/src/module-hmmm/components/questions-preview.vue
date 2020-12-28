<template>
  <div class="container">
    <!-- 弹出对话框 -->
    <el-dialog
      title="题目预览"
      :visible.sync="previewDialogVisible"
      width="60%"
      :show-close="false"
    >
      <div class="Dialog_preview">
        <el-row>
          <el-col :span="6" v-if="querestItem.questionType">
            <span>【题型】:{{ questionType[querestItem.questionType].label }}</span>
          </el-col>
          <el-col :span="6">
            <span>【编号】：{{ querestItem.id }}</span>
          </el-col>
          <el-col :span="6" v-if="querestItem.questionType">
            <span>【难度】：{{ difficulty[querestItem.difficulty].label }}</span>
          </el-col>
          <el-col :span="6">
            <span>【标签】：{{ querestItem.tags }}</span>
          </el-col>
        </el-row>
        <el-row>
          <el-col :span="6">
            <span>【目录】：{{ querestItem.directoryName }}</span>
          </el-col>
          <el-col :span="6">
            <span>【方向】：{{ querestItem.direction }}</span>
          </el-col>
          <el-col :span="6">
            <span>【学科】：{{ querestItem.subjectName }}</span>
          </el-col>
        </el-row>
        <hr />
        <div>【题干】： <span v-html="querestItem.question"></span></div>
        <div class="querest-item">
          <pre></pre>
        </div>
        <span>单选题 选项：（以下选中的选项为正确答案）</span>
        <div>
          <div>
            <!-- <el-checkbox
              v-model="checked"
              v-for="(item, index) in querestItem.options"
              style="padding: 8px 0px;"
            >
          <template v-if="checked=true ? 'item.isRight===1' : ''"></template>
                {{ item.code }}:<span>{{ item.title }}</span>
          </template>
            </el-checkbox> -->

            <el-checkbox
              style="display: block; padding:8px 0"
              :checked="item.isRight === 1"
              v-for="(item, index) in querestItem.options"
              :key="item.id"
              >{{ item.code }}:<span>{{ item.title }}</span>
            </el-checkbox>
          </div>
          <!-- <div>
            <el-checkbox v-model="checked"  style="padding: 8px 0px;">备选项</el-checkbox>
          </div>
          <div>
            <el-checkbox v-model="checked"  style="padding: 8px 0px;">备选项</el-checkbox>
          </div>
          <div>
            <el-checkbox v-model="checked"  style="padding: 8px 0px;">备选项</el-checkbox>
          </div> -->
          <hr />
          【参考答案】：
          <el-button type="danger" size="small" @click="clickShow">
            <span>视频答案预览</span>
          </el-button>
          <div class="video" v-show="isShow">
            <video controls="controls" :src="querestItem.videoURL" class="video-shipin"></video>
          </div>
          <hr />
          【答案解析】: 测试
          <hr />
          【题目备注】: 测试
        </div>
      </div>
      <div style="text-align: right;">
        <span slot="footer" class="dialog-footer">
          <el-button type="primary" @click="$emit('close')">关闭</el-button>
        </span>
      </div>
    </el-dialog>
  </div>
</template>

<script>
import { questionType, direction, difficulty } from '@/api/hmmm/constants'

export default {
  props: {
    value: {
      type: [Array, String, Object],
      required: true
    },
    previewDialogVisible: {
      type: [Boolean, String],
      // default:'500' //默认值，如果父级没有传递时候，会采用默认值
      required: true ////必须传递 不能和default同时引用
    },
    querestItem: {
      type: [Object, String],
      required: true
    }
  },
  data() {
    return {
      checked: '',
      checked: false,
      isShow: false,
      questionType: questionType,
      direction: direction,
      difficulty: difficulty,
      checkedMoren: ''
    }
  },

  methods: {
    clickShow() {
      if (!this.isShow) {
        this.isShow = true
      } else {
        this.isShow = false
      }
    }
  }
}
</script>

<style scoped lang="less">
.Dialog_preview {
  padding: 30px 20px;
  .el-row {
    padding: 30px 20px;
    color: #606266;
    font-size: 14px;
    word-break: break-all;
  }
  .video {
    padding: 30px 0;
    width: 400px;
    height: 300px;
    .video-shipin {
      width: 100%;
      height: 100%;
    }
  }

  /* border-bottom: 1px solid #ccc; */
}
</style>
