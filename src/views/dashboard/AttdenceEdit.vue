<template>
  <div class="app-container">
    <div class="table_center">
      <table class="content">
        <tr>
          <td
            colspan="6"
            style="font-size: 24px; font-weight: bold; color: #304156"
          >
            {{formName}}
          </td>
        </tr>
        <tr style="height: 40px">
          <td colspan="1" width="200">学号</td>
          <td colspan="5">
            <el-input
              v-model="form.studentNum"
              placeholder="请输入学号"
              style="width: 90%"
            />
          </td>
        </tr>

        
        <tr style="height: 40px">
          <td colspan="1" width="200">姓名</td>
          <td colspan="5">
            <el-input
              v-model="form.studentName"
              placeholder="请输入姓名"
              style="width: 90%"
            />
          </td>
        </tr>

        <tr style="height: 40px">
          <td colspan="1" width="200">课程序号</td>
          <td colspan="5">
            <el-input
              v-model="form.courseNum"
              placeholder="请输入"
              style="width: 90%"
            />
          </td>
        </tr>
         
         <tr style="height: 40px">
          <td colspan="1" width="200">课程名称</td>
          <td colspan="5">
            <el-input
              v-model="form.courseName"
              placeholder="请输入"
              style="width: 90%"
            />
          </td>
        </tr>

        <tr style="height: 40px">
          <td colspan="1" width="200">选择</td>
          <td colspan="5">
            <el-select
              v-model="form.attdence"
              placeholder="请选择完成"
              style="width: 90%"
            >
              <el-option
                v-for="items in attdenceList"
                :key="items.value"
                :label="items.label"
                :value="items.value"
              >
              </el-option>
            </el-select>
          </td>
        </tr>

      </table>
    </div>

    <div class="centerButton">
      <el-button size="mini" class="rowButton" @click="doSumit()"
        >提交</el-button
      >
    </div>
  </div>
</template>

<script>
import { attdenceEditInit,attdenceEditSubmit,getAuthHeader, getPersonImage } from "@/service/genServ.js";
export default {
  name: "AttdenceEdit",
  data() {
    return {
      form: {
        studentNum: "",
        studentName: "",
        courseNum: "",
        courseName: "",
        attdence: "",
      },
      authHeader: getAuthHeader(),
      image0: "",
      attdenceList:[],
      formName: "出勤信息",
    };
  },
  created() {
    this.id = this.$route.query.id;
    this.fetchData();
  },
  methods: {
    fetchData() {
      attdenceEditInit({ id: this.id }).then((res) => {
        this.form = res.data.form;
        this.attdenceList = res.data.attdenceList;
        this.image0 = res.data.image0;
      });
    },

    doSumit() {
      attdenceEditSubmit({ id: this.id, form: this.form }).then(
        (res) => {
          if (res.code === 0) {
            this.$message({
              message: '保存成功！',
              type: 'warnning',
            })            
            this.id = res.data.id;
          }else {
            this.$message({
              message: res.msg,
              type: 'warnning',
            })            
          }
        }
      );
    },
    onSuccess(response, file, fileList) {
      if (response.code == 0) {
        this.$message({
          message: "上传成功",
          type: "success",
        });
        getPersonImage({ studentNum: this.form.studentNum, no: "0" }).then((res) => {
          this.image0 = res.data;
        });
      }
      this.$emit("upLoadSucess", response, file, fileList);
    },
  },
};
</script>