<template>
  <div class="container">
    <!-- 弹出对话框 -->
    <el-dialog title="题目预览" :visible.sync="previewDialogVisible" width="60%">
      <div class="Dialog_preview">
        <el-row>
          <el-col :span="6">
            <span>【题型】:{{questionType[querestItem.questionType].label}}</span>
          </el-col>
          <el-col :span="6">
            <span>【编号】：{{querestItem.id}}</span>
          </el-col>
          <el-col :span="6">
            <span>【难度】：{{difficulty[querestItem.difficulty].label}}</span>
          </el-col>
          <el-col :span="6">
            <span>【标签】：{{querestItem.tags}}</span>
          </el-col>
        </el-row>
        <el-row>
          <el-col :span="6">
            <span>【目录】：{{querestItem.directoryName}}</span>
          </el-col>
          <el-col :span="6">
            <span>【方向】：{{querestItem.direction}}</span>
          </el-col>
          <el-col :span="6">
            <span>【学科】：{{querestItem.subjectName}}</span>
          </el-col>
        </el-row>
        <hr />
        【题干】： <span v-html="querestItem.question"></span>
        <div class="querest-item">
          <pre></pre>
        </div>
        <span>单选题 选项：（以下选中的选项为正确答案）</span>
        <div>
          <div>
            <el-checkbox v-model="checked"  style="padding: 8px 0px;">备选项</el-checkbox>
          </div>
          <div>
            <el-checkbox v-model="checked"  style="padding: 8px 0px;">备选项</el-checkbox>
          </div>
          <div>
            <el-checkbox v-model="checked"  style="padding: 8px 0px;">备选项</el-checkbox>
          </div>
          <div>
            <el-checkbox v-model="checked"  style="padding: 8px 0px;">备选项</el-checkbox>
          </div>
          <hr />
          【参考答案】：
          <el-button type="danger" size="small" @click="clickShow">
            <span>视频答案预览</span>
          </el-button>
          <div class="video" v-show="isShow">
            <video controls="controls" src="" class="video-shipin"></video>
          </div>
          <hr />
          【答案解析】: 测试
          <hr />
          【题目备注】: 测试
        </div>
      </div>
      <div style="text-align: right;">
        <span slot="footer" class="dialog-footer">
          <el-button type="primary" @click="previewDialogVisible = false">关闭</el-button>
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
      isShow: false,
      questionType: questionType,
      direction: direction,
      difficulty: difficulty
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
