<template>
    <div class="base_form" style="padding-top: 15px">
      <div class="base_query_oneLine">
        <div class="query_left">
        <el-button class="orangeButton" size="small" @click="addItem()"
          >添加</el-button
        >
        </div>
        <div class="query_right">
          <el-input class="marginl"
            placeholder="请输入课程号，课程名称，学生名或学号"
            v-model="numName"
            style="width: 300px; margin-right: 5px"
          />
          <el-button class="blueButton" size="small" @click="doQuery()"
            >查询</el-button
          >
        </div>
      </div>
      <div class="table-content">
        <el-table :data="dataList" border style="width: 100%">
          <el-table-column prop="studentNum" label="学号" width="180"> </el-table-column>
          <el-table-column prop="studentName" label="学生姓名" width="180"> </el-table-column>
          <el-table-column prop="courseNum" label="课程号" width="180"> </el-table-column>
          <el-table-column prop="courseName" label="课程名" width="180"> </el-table-column>
          <el-table-column prop="score" label="成绩"> </el-table-column>
          <el-table-column label="操作" width="200" align="center" color="black">
            <template v-slot="scope">
              <el-button
                class="table_edit_button"
                @click="doEdit(scope.row.id)"
                size="mini"
                >编辑</el-button
              >
              <el-button
                class="table_delete_button"
                style="margin-left: 5px"
                @click="deleteRow(scope.row.id)"
                size="mini"
                >删除</el-button
              >
            </template>
          </el-table-column>
        </el-table>
      </div>
    </div>
  </template>
  
  <script>
  import { scoreInit,scoreQuery,scoreDelete } from "@/service/genServ.js";
  export default {
    name: "ScoreTable",
    data() {
      return {
        numName:'',
        dataList: [],
      };
    },
    created() {
        scoreInit({}).then(
          (res) => {
            this.dataList = res.data;
          }
        );
    },
    methods: {
      doQuery() {
        scoreQuery({numName:this.numName}).then((res) => {
            if(res.code === 0) {
              this.dataList = res.data;
            }else {
              this.$message({
                message: res.msg,
                type: 'warnning',
              })            
            }
        });
      },
      doEdit(id) {
        this.$router.push({
          path: "ScoreEdit",
          query: { id: id },
        });
      },
      addItem() {
        this.$router.push({
          path: "ScoreEdit",
          query: {},
        });
      },
      deleteRow(id) {
        scoreDelete({ id: id }).then((res) => {
          if (res.code == 0) this.doQuery();
          else{
              this.$message({
                message: res.msg,
                type: 'warnning',
              })            
          }
        });
      },
  
    },
  };
  </script>