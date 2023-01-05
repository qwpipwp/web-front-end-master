
<template>
    <div ref="imageTofile" class="PDF">
        <el-row :gutter="0" class="form" style="display: flex " v-if="showstu">
            <el-col :span="18">
                <el-descriptions :column="2" border>
                    <el-descriptions-item label="学生姓名">{{ form.studentName }}</el-descriptions-item>
                    <el-descriptions-item label="学号">{{ form.studentNum }}</el-descriptions-item>
                    <el-descriptions-item label="性别">{{ getSex(form.sex) }}</el-descriptions-item>
                    <el-descriptions-item label="学院">{{ form.dept }}</el-descriptions-item>
                    <el-descriptions-item label="出生日期">{{ form.birthday }}</el-descriptions-item>
                    <el-descriptions-item label="年龄">{{ form.age }}</el-descriptions-item>
                </el-descriptions>
            </el-col>
            <el-col :span="6" style="align-items: stretch">
                <div class="imgBox">
                    <img class="devImg" fit="fill" :alt="this.username" :src="'/md/'+ this.username+ '/头像.jpg'">
                    <!-- <img class="devImg" :src="require(`../../public/md/${this.username}/头像.jpg`)" /> -->
                    <!-- <img class="devImg" fit="fill" :src="require('@/assets/1.jpg')" /> -->
                </div>
            </el-col>
        </el-row>
        <el-row :gutter="0" style="display: flex" v-if="!showstu">
            <el-col :span="18">
                <el-descriptions :column="2" border>
                    <el-descriptions-item label="老师姓名">{{ form.studentName }}</el-descriptions-item>
                    <el-descriptions-item label="性别">{{ getSex(form.sex) }}</el-descriptions-item>
                    <el-descriptions-item label="学院">{{ form.dept }}</el-descriptions-item>
                    <el-descriptions-item label="出生日期">{{ form.birthday }}</el-descriptions-item>
                    <el-descriptions-item label="年龄">{{ form.age }}</el-descriptions-item>
                </el-descriptions>
            </el-col>
            <el-col :span="6" style="align-items: stretch">
                <div class="imgBox">
                    <img class="devImg" fit="fill" :alt="this.username" :src="'/md/'+ this.username+ '/头像.jpg'">
                </div>
            </el-col>
        </el-row>
        <v-md-editor v-model="text" height="600px" mode="preview"></v-md-editor>
    </div>
    <el-button class="button-dalod" size="mini" title="生成图片" @click="getPDF()" icon="el-icon-download"></el-button>
    
</template>

  
<script>
import axios from 'axios';
import htmlToPdf from "@/utils/getPDF.js";
import { studentEditInit, getAuthHeader, } from "@/service/genServ.js";
import { mapState } from 'vuex';
export default {
    name: "Information",
    data() {
        return {
            text: '',
            form: {
                studentNum: "",
                studentName: "",
                sex: "",
                age: "",
                dept: "",
                birthday: "",
                text: '',
            },
            authHeader: getAuthHeader(),
            image0: "",
            sexList: [],
            formName: "个人信息",
            identity: '',
            showstu: '',
        };
    },
    computed: {
        ...mapState([
            'username', 'role'
        ])
    },
    methods: {
        fetchData() {
            studentEditInit({ id: this.id }).then((res) => {
                this.form = res.data.form;
                this.sexList = res.data.sexList;
                this.image0 = res.data.image0;
            });
        },
        getSex(sex) {
            if (sex == "1") {
                this.sex = "男";
            } else {
                this.sex = "女";
            }
            return this.sex;
        },
        getPDF() {
            htmlToPdf.downloadPDF(document.querySelector(".PDF"), "个人主页");
        },
    },
    mounted() {
        if (this.role == '老师') {
            this.identity = 'teacher'
            this.showstu = false
        }
        else {
            this.identity = 'student'
            this.showstu = true
        }
        //获得md文件渲染到页面
        axios.get(`/md/${this.identity}/intro.md`).then(res => {
            this.text = res.data;
        })

    },
    created() {
    }
};
</script>

<style>


.imgBox {
    border: 1px solid #ebeef5;
    border-left: none;
    width: 250px;
    height: 250px;
    position: relative;
}

.devImg {
    background-size: contain;
    max-width:100%;
    max-height:100%;
}

.form {
    margin-top: 10px;
}
</style>