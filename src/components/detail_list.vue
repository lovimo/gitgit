<template >
  <el-table :data="studentData" border style="width: 100%">
  <el-table-column prop="id" label="学号"></el-table-column>
  <el-table-column prop="name" label="姓名"></el-table-column>
  <el-table-column prop="age" label="年龄"></el-table-column>
  <el-table-column prop="sex" label="性别"></el-table-column>
  <el-table-column prop="major" label="专业"></el-table-column>
  <el-table-column prop="depart" label="院系"></el-table-column>
  <el-table-column prop="term" label="学期"></el-table-column>
  <el-table-column prop="year" label="年级"></el-table-column>
  <el-table-column prop="role" label="角色"></el-table-column>
  <el-table-column label="操作">
    <template scope="scope">
      <el-button type="primary" size="small" @click="studentEdit(scope.$index, scope.row)">编辑</el-button>
      <el-button type="danger" size="small" @click="studentDelete(scope.row)">删除</el-button>     //scope.row代表当前对应行
    </template>
  </el-table-column>
</el-table>
<div class="block" style="height:70px;">
  <el-pagination
    @size-change="sizeChange"
    @current-change="currentChange"
    :page-sizes="[10,20,30,40]"
    :page-size="page.pageSize"
    layout="total, sizes, prev, pager, next"
    :total="page.totalRecords">
  </el-pagination>
</div>
//新增学生信息模态框
<el-dialog title="新增学生信息" :visible="addstudentForm" size="tiny" :modal-append-to-body='false' @close='closeDialog'>
  <el-form id="#addsForm" ref="addsForm" :model="addsForm" label-width="80px">
    <el-form-item label="学号" prop="id">
      <el-input  v-model="addsForm.id" max-length="10"></el-input>
    </el-form-item>
    <el-form-item label="姓名" prop="name">
      <el-input v-model="addsForm.name"></el-input>
    </el-form-item>
    <el-form-item label="年龄" prop="age">
      <el-input v-model="addsForm.age"></el-input>
    </el-form-item>
    <el-form-item label="性别" prop="sex">
      <el-input v-model="addsForm.sex"></el-input>
    </el-form-item>
    <el-form-item label="专业" prop="major">
      <el-input v-model="addsForm.major"></el-input>
    </el-form-item>
    <el-form-item label="院系" prop="depart">
      <el-input v-model="addsForm.depart"></el-input>
    </el-form-item>
    <el-form-item label="学期">
      <el-select v-model="addsForm.term" value-key="id">
        <el-option v-for="item in termArry"  :key="item.id" :label="item.name" :value="item.id"></el-option>
      </el-select>
    </el-form-item>
    <el-form-item label="年级" prop="year">
      <el-input v-model="addsForm.year"></el-input>
    </el-form-item>
    <el-form-item label="角色" prop="role">
      <el-input v-model="addsForm.role" disabled="disabled"></el-input>
    </el-form-item>
    <el-form-item>
      <el-button type="primary" @click="studentAdd()">确定</el-button>
      <el-button @click="addstudentForm = false;canceladdT('formt')">取消</el-button>
    </el-form-item>
  </el-form>
</el-dialog>

//编辑学生信息模态框
<el-dialog title="编辑学生信息" :visible="editstudentForm" size="tiny" :modal-append-to-body='false' @close='closeDialog'>
  <el-form ref="editsForm" :model="editsForm" label-width="80px">
    <el-form-item label="学号">
      <el-input  v-model="editsForm.id" max-length="10" disabled="disabled"></el-input>
    </el-form-item>
    <el-form-item label="姓名">
      <el-input v-model="editsForm.name"></el-input>
    </el-form-item>
    <el-form-item label="年龄">
      <el-input v-model="editsForm.age"></el-input>
    </el-form-item>
    <el-form-item label="性别">
      <el-input v-model="editsForm.sex"></el-input>
    </el-form-item>
    <el-form-item label="专业">
      <el-input v-model="editsForm.major"></el-input>
    </el-form-item>
    <el-form-item label="院系">
      <el-input v-model="editsForm.depart"></el-input>
    </el-form-item>
    <el-form-item label="学期">
      <el-select v-model="editsForm.term" value-key="id">
        <el-option v-for="item in termArry"  :key="item.id" :label="item.name" :value="item.id"></el-option>
      </el-select>
    </el-form-item>
    <el-form-item label="年级">
      <el-input v-model="editsForm.year"></el-input>
    </el-form-item>
    <el-form-item label="角色">
      <el-input v-model="editsForm.role" disabled="disabled"></el-input>
    </el-form-item>
    <el-form-item label="密码">
      <el-input v-model="editsForm.passwd" disabled="disabled"></el-input>
    </el-form-item>
    <el-form-item>
      <el-button type="primary" @click="studentcEdit()">确定</el-button>
      <el-button @click="editstudentForm = false">取消</el-button>
    </el-form-item>
  </el-form>
</el-dialog>


</template>
 
<script>
 export default{
data(){
return{
　　studentData:[],  //所有学生信息数组置空
addstudentForm:false, //新增学生信息模态框
page: {
  pageSize: 10, //每页条数,  默认10条
  totalRecords: 0, //总条数
  totalPages: 0, //总页数
  pageNum:0
},
addsForm:{
  id:'',
  name:'',
  age:"",
  sex:'',
  major:'',
  depart:'',
  term:'',
  year:'',
  role:'0'
},
editsForm:{
  id:'',
  name:'',
  age:"",
  sex:'',
  major:'',
  depart:'',
  term:'',
  year:'',
  role:'0',
  passwd:''
},

}
;
mounted(){
　　this.init(); //页面内初始加载就调用这个函数
}
methods:{
init(){
this.studentData = [];
let {pageNum,pageSize} = this.page; //es6写法
 // pageNum:页数从0开始
//pageSize:每页显示10条

this.$http.get(Main.getStudent(pageNum,pageSize)).then(res =>{
  let {errCode,errMsg}=res.data;
  this.page.totalRecords=res.data.totalRecords; //总条数
  if(errCode==0){
    const studentArray=res.data.dataList;
    this.studentData=studentArray;
  }else{
      alert(errMsg);
  }
}, response => {
})

}
,
// 每页显示多少条数据
sizeChange(val) {
  this.page.pageSize = val;
  this.init();
},
//翻页
currentChange(val) {
  this.page.pageNum=val-1;
  console.log(this.page.pageNum);
  this.init();
},

// 点击模态框关闭按钮关闭模态框
closeDialog(){
  this.addstudentForm = false;
  this.editstudentForm = false;
},
//新增数据条数
//新增学生信息后台提交参数
</script>
 
<style scoped>
  .el-dropdown {
    vertical-align: top;
  }
  .el-dropdown + .el-dropdown {
    margin-left: 15px;
  }
  .el-icon-arrow-down {
    font-size: 12px;
  }
  .headerWrap{
    height: 36px;
    line-height: 36px;
  }
  .tablenameWrap{
    width: 150px;
  }
  .updateDataWrap{
    margin: 20px 0;
  }
  .pagination{
    float: right;
    margin-top: 20px;
  }
</style>
 
