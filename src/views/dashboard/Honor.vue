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
          placeholder="请输入学号或姓名"
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
      <el-table-column prop="id" label="id"> </el-table-column>
        <el-table-column prop="studentNum" label="学号" width="180"> </el-table-column>
        <el-table-column prop="studentName" label="姓名" width="180"> </el-table-column>
        
        <el-table-column prop="honor" label="荣誉"> </el-table-column>
       
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
import { honorInit,honorQuery,honorDelete } from "@/service/genServ.js";
export default {
  name: "Honor",
  data() {
    return {
      numName:'',
      dataList: [],
    };
  },
  created() {
    honorInit({}).then(
        (res) => {
          this.dataList = res.data;
        }
      );
  },
  methods: {
    doQuery() {
      honorQuery({numName:this.numName}).then((res) => {
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
        path: "HonorEdit",
        query: { id: id },
      });
    },
    addItem() {
      this.$router.push({
        path: "HonorEdit",
        query: {},
      });
    },
    deleteRow(id) {
     honorDelete({ id: id }).then((res) => {
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