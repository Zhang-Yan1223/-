<template>
  <div style="padding:1em">
    <!-- 按钮
    表格
    分页
    模态框 -->
    <el-button type="primary" size="small" @click="toAddHandlar">添加</el-button>
    <el-button type="danger" size="small">批量删除</el-button>
    <el-table :data="employees">
      <el-table-column prop="id" label="编号"></el-table-column>
      <el-table-column prop="name" label="姓名"></el-table-column>
      <el-table-column prop="gender" label="性别"></el-table-column>
      <el-table-column prop="telephone" label="手机号"></el-table-column>
      <el-table-column prop="idcard" label="身份证号"></el-table-column>
      <el-table-column prop="yincard" label="银行卡号"></el-table-column>
      <el-table-column label="操作">
        <template v-slot="slot">
          <a href="" @click.prevent="toDeleteHandle(slot.row.id)">删除</a>
          <a href="" @click.prevent="toupdateHandler(slot.row.id)">修改</a>
        </template>
      </el-table-column>
    </el-table>
    <el-pagination
    background
    layout="prev, pager, next"
    :total="50">
    </el-pagination>
    <!-- 分页 -->
    <el-dialog
        :title="title"
        :visible.sync="visible"
        width="60%">
        {{form}}
        <!-- :before-close="handleClose" -->
        <!-- <span>这是一段信息</span> -->
        <el-form :model="form"  label-width="80px">
          <el-form-item label="用户名">
            <el-input v-model="form.username"/>
          </el-form-item>
          <el-form-item label="密码" >
            <el-input v-model="form.password" type="password"/>
          </el-form-item>
          <el-form-item label="身份证号">
            <el-input v-model="form.idcard" />
          </el-form-item>
          <el-form-item label="银行卡号">
            <el-input v-model="form.bankcard" />
          </el-form-item>
          <el-form-item label="性别">
            <el-radio-group v-model="form.gender">
              <el-radio label = "男">男</el-radio>
              <el-radio label = "女">女</el-radio>
            </el-radio-group>
          </el-form-item>
        </el-form>
        <span slot="footer" class="dialog-footer">
            <el-button @click="closeModelHandler">取消</el-button>
            <el-button type="primary" @click="closeModelHandler">确定</el-button>
        </span>
    </el-dialog>
    <!-- 模态框 -->
  </div> 
</template>
<script>
import request from "@/utils/request";
import querystring from 'querystring';
export default {
  data() {
    return {
      title: '录入员工信息',
      visible: false,
      employees: [{
        // id: 1,
        // name: '晴慕雪',
        // gender: '女',
        // telephone: '1233414214',
        // idcard: '1234455312',
        // yincard: '1243531242'
      }],
      form:{
        type:"waiter"
      },
    }
  },
  created(){
    // 页面加载出来的时候加载数据
    this.loadDate()
  },
  methods: {
    loadDate(){
      let url = 'http://localhost:6677/waiter/findAll'
      request.get(url).then((response)=>{
        // 箭头函数中的this指向外部函数的this
        this.employees = response.data
      })
    },
    submitHandle() {
      //通过request与后台进行交互
      let url = 'http://localhost:6677/waiter/saveOrUpdate'
      // request.post(url.this.form);
      request({
        url,
        method : "post",
        headers:{
          "Content-Type":"application/x-www-form-urlencoded"
        },
        data : querystring.stringify(this.form)
      }).then((response)=>{
        //请求结束
        this.closeModelHandler();
        this.loadDate();
        this.$message({
          type:"success",
          $message:response.$message,
        })
      })
    },
    toDeleteHandle(id) {
      this.$confirm('确定删除这段数据？', {
        confirmButtonText: '是的',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        this.$message({
          type: 'success',
          message: '已删除' + id
        })
      })
    },
    toupdateHandler() {
      this.title = '修改员工信息'
      this.visible = true
    },
    closeModelHandler() {
      this.visible = false
    },
    toAddHandlar() {
      this.visible = true
    }
  }
}
</script>
<style scoped>
</style>
