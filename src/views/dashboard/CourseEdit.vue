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
            <td colspan="1" width="200">课程号</td>
            <td colspan="5">
              <el-input
                v-model="form.courseNum"
                placeholder="请输入课程号"
                style="width: 90%"
              />
            </td>
          </tr>
  
          <tr style="height: 40px">
            <td colspan="1" width="200">课程名称</td>
            <td colspan="5">
              <el-input
                v-model="form.courseName"
                placeholder="请输入课程名称"
                style="width: 90%"
              />
            </td>
          </tr>
          <tr style="height: 40px">
            <td colspan="1" width="200">学分</td>
            <td colspan="5">
              <el-input
                v-model="form.credit"
                placeholder="请输入学分"
                style="width: 90%"
              />
            </td>
          </tr>
          <tr style="height: 40px">
            <td colspan="1" width="200">预选课</td>
            <td colspan="5">
              <el-input
                v-model="form.precourse"
                placeholder="请输入预选课名称"
                style="width: 90%"
              />
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
  import { courseEditInit,courseEditSubmit,getAuthHeader} from "@/service/genServ.js";
  export default {
    name: "CourseEdit",
    data() {
      return {
        form: {
          courseNum: "",
          courseName: "",
          credit: "",
          precourse: "",
        },
        authHeader: getAuthHeader(),
        formName: "课程信息",
      };
    },
    created() {
      this.id = this.$route.query.id;
      this.fetchData();
    },
    methods: {
      fetchData() {
        courseEditInit({ id: this.id }).then((res) => {
          this.form = res.data.form;
        });
      },
  
      doSumit() {
        courseEditSubmit({ id: this.id, form: this.form }).then(
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
      
    },
  };
  </script>