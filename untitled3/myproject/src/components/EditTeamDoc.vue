<template>
  <div>
    <el-container>
      <el-header>
        <NavBar style="z-index: 9999"></NavBar>
      </el-header>
      <el-main>
        <el-card class="main-body">
          <div class="card">
            <el-row>
              <p class="top-title">创建文档</p>
            </el-row>
            <div class="tips">
              <p>你可以使用“模板”功能来快速创建文档</p>
            </div>
            <div style="margin-top: 40px;">
              <el-form :model="docForm" label-width="80px" :rules="rule" ref="docForm">
                <el-row>
                  <el-form-item label="文档标题" prop="title">
                    <el-input v-model="docForm.title"></el-input>
                  </el-form-item>
                </el-row>
                <p style="margin-left: 12px; font-size: 14px">文档权限：</p>
                <el-row>
                  <el-col span="6">
                    <el-form-item label="查看" prop="viewP">
                      <el-select v-model="docForm.viewP" placeholder="请选择">
                        <el-option label="不公开" value="0"></el-option>
                        <el-option label="仅团队" value="2"></el-option>
                        <el-option label="所有人" value="1"></el-option>
                      </el-select>
                    </el-form-item>
                  </el-col>
                  <el-col span="6">
                    <el-form-item label="评论" prop="commentP">
                      <el-select v-model="docForm.commentP" placeholder="请选择">
                        <el-option label="不公开" value="0"></el-option>
                        <el-option label="仅团队" value="2"></el-option>
                        <el-option label="所有人" value="1"></el-option>
                      </el-select>
                    </el-form-item>
                  </el-col>
                  <el-col span="6">
                    <el-form-item label="编辑" prop="editP">
                      <el-select v-model="docForm.editP" placeholder="请选择">
                        <el-option label="不公开" value="0"></el-option>
                        <el-option label="仅团队" value="2"></el-option>
                        <el-option label="所有人" value="1"></el-option>
                      </el-select>
                    </el-form-item>
                  </el-col>
                  <el-col span="6">
                    <el-form-item label="分享" prop="shareP">
                      <el-select v-model="docForm.shareP" placeholder="请选择">
                        <el-option label="不公开" value="0"></el-option>
                        <el-option label="仅团队" value="2"></el-option>
                        <el-option label="所有人" value="1"></el-option>
                      </el-select>
                    </el-form-item>
                  </el-col>
                </el-row>
                <div class="editor">
                  <vue-ueditor-wrap v-model="docForm.doc" :config="ueConfig"></vue-ueditor-wrap>
                </div>

                <el-form-item class="button-row">
                  <el-button type="primary" @click="onSubmit('docForm')">提交</el-button>
                  <el-button style="margin-left: 30px">取消</el-button>
                </el-form-item>
              </el-form>
            </div>
          </div>
        </el-card>
      </el-main>
    </el-container>
    <el-backtop right="80"></el-backtop>
  </div>
</template>

<script>
import UEditor from "./UEditor";
import NavBar from "./NavBar";
import VueUeditorWrap from "vue-ueditor-wrap";
import axios from "axios";
export default {
  name: "EditTeamDoc",
  components: { UEditor, NavBar, VueUeditorWrap },
  data() {
    return {
      headUrl: require("../assets/head.jpg"),
      docForm: {
        title: "",
        doc: "我是默认内容咿呀咿呀咿",
        viewP: "1",
        commentP: "1",
        editP: "1",
        shareP: "1",
      },
      rule: {
        title: [
          { required: true, message: "请输入标题", trigger: "blur" },
          { max: 25, message: "标题长度在25字以内", trigger: "change" },
        ],
        viewP: [{ required: true, message: "请选择权限", trigger: "change" }],
        commentP: [
          { required: true, message: "请选择权限", trigger: "change" },
        ],
        editP: [{ required: true, message: "请选择权限", trigger: "change" }],
        shareP: [{ required: true, message: "请选择权限", trigger: "change" }],
      },
      ueConfig: {
        toolbars: [
          [
            "source", // 源代码
            // 'anchor', // 锚点
            "undo", // 撤销
            "redo", // 重做
            "bold", // 加粗
            "indent", // 首行缩进
            // 'snapscreen', //截图
            "italic", // 斜体
            "underline", // 下划线
            "strikethrough", // 删除线
            "subscript", // 下标
            "fontborder", // 字符边框
            "superscript", // 上标
            "formatmatch", // 格式刷
            "blockquote", // 引用
            "pasteplain", // 纯文本粘贴模式
            "selectall", // 全选
            // 'print', // 打印
            "preview", // 预览
            "horizontal", // 分隔线
            "removeformat", // 清除格式
            // 'time', // 时间
            // 'date', // 日期
            // 'unlink', // 取消链接
            "insertrow", // 前插入行
            "insertcol", // 前插入列
            "mergeright", // 右合并单元格
            "mergedown", // 下合并单元格
            "deleterow", // 删除行
            "deletecol", // 删除列
            "splittorows", // 拆分成行
            "splittocols", // 拆分成列
            "splittocells", // 完全拆分单元格
            "deletecaption", // 删除表格标题
            "inserttitle", // 插入标题
            "mergecells", // 合并多个单元格
            "deletetable", // 删除表格
            "cleardoc", // 清空文档
            "insertparagraphbeforetable", // "表格前插入行"
            "insertcode", //代码语言
            "fontfamily", // 字体
            "fontsize", // 字号
            // 'paragraph', // 段落格式
            "simpleupload", // 单图上传
            //'insertimage', // 多图上传
            "edittable", // 表格属性
            "edittd", // 单元格属性
            // 'link', // 超链接
            // 'emotion', // 表情
            // 'spechars', // 特殊字符
            // 'searchreplace', // 查询替换
            // 'map', // Baidu地图
            // 'gmap', // Google地图
            // 'insertvideo', // 视频
            // 'help', // 帮助
            "justifyleft", // 居左对齐
            "justifyright", // 居右对齐
            "justifycenter", // 居中对齐
            "justifyjustify", // 两端对齐
            "forecolor", // 字体颜色
            "backcolor", // 背景色
            "insertorderedlist", // 有序列表
            "insertunorderedlist", // 无序列表
            // 'fullscreen', // 全屏
            "directionalityltr", // 从左向右输入
            "directionalityrtl", // 从右向左输入
            "rowspacingtop", // 段前距
            "rowspacingbottom", // 段后距
            // 'pagebreak', // 分页
            // 'insertframe', // 插入Iframe
            "imagenone", // 默认
            "imageleft", // 左浮动
            "imageright", // 右浮动
            // 'attachment', //附件
            "imagecenter", // 居中
            // 'wordimage', //图片转存
            "lineheight", // 行间距
            // 'edittip ', // 编辑提示
            // 'customstyle', // 自定义标题
            "autotypeset", // 自动排版
            // 'webapp', // 百度应用
            "touppercase", // 字母大写
            "tolowercase", // 字母小写
            // 'background', // 背景
            "template", // 模板
            // 'scrawl', // 涂鸦
            // 'music', // 音乐
            "inserttable", // 插入表格
            // 'drafts', // 从草稿箱加载
            // 'charts', // 图表
          ],
        ],
        // 初始容器高度
        initialFrameHeight: 400,
        // 初始容器宽度
        initialFrameWidth: "100%",
        // 上传文件接口
        enableAutoSave: true,
        autoHeightEnabled: false,
        serverUrl: "http://175.24.74.107:8081/ueditor/configs",
        // serverUrl: "http://127.0.0.1:8081",
      },
    };
  },
  methods: {
    onSubmit(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          var _this = this;
          var userL = JSON.parse(sessionStorage.getItem("userL"));
          var pri = 0;
          if(this.docForm.viewP === '1'){
            pri += 1000;
          }
          else if(this.docForm.viewP === '2'){
            pri += 2000;
          }
          if(this.docForm.editP === '1'){
            pri += 100;
          }
          else if(this.docForm.editP === '2'){
            pri += 200;
          }
          if(this.docForm.commentP === '1'){
            pri += 10;
          }
          else if(this.docForm.commentP === '2'){
            pri += 20;
          }
          if(this.docForm.shareP === '1'){
            pri += 1;
          }
          else if(this.docForm.shareP === '2'){
            pri += 2;
          }
          console.log('pri=' + pri);
          axios
            .post("/doc", {
              docID: this.$route.params.id,
              userID: userL.userID,
              title: this.docForm.title,
              content: this.docForm.doc,
              privilege: pri,
              isTeam: 1,
              team: _this.$route.params.tid,
            })
            .then(function (response) {
              // console.log(response.data.status)
              if (response.data.status === 200) {
                //alert("新建文档成功")
                _this.$message({
                  message: "新建文档成功",
                  type: "success",
                });
                // _this.$router.push('/detail/' + response.data.data)
                _this.$router.push({
                  path: "/detail",
                  query: {
                    key: _this.$Base64.encode(
                      JSON.stringify({
                        id: response.data.data,
                      })
                    ),
                  },
                });
              }
            })
            .catch(function (error) {
              console.log(error);
            });
        } else {
          console.log("error submit!!");
          return false;
        }
      });
    },
    getContent() {
      var _this = this;
      axios
        .post("/template/" + this.$route.params.template)
        .then(function (response) {
          var content = response.data;
          _this.docForm.doc = content;
        })
        .catch(function (error) {
          // 请求失败处理
          console.log(error);
        });
    },
  },
  created() {
    this.getContent();
  },
};
</script>

<style scoped>
.main-body {
  width: 90%;
  margin-left: 5%;
}
.top-title {
  float: left;
  font-size: 30px;
  color: #333333;
}
.button-row {
  text-align: center;
  margin-top: 30px;
  width: 100%;
}
.tips {
  border-radius: 5px;
  background: RGB(236, 248, 255);
  padding: 12px;
  margin-bottom: 20px;
  border: 5px solid RGB(236, 248, 255);
  border-left-color: RGB(80, 191, 255);
}
.editor {
  text-align: center;
}
.card {
  width: 90%;
  margin-left: 5%;
}
</style>
