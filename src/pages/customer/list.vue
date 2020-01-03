<template>
  <div style="padding:1em">
    <el-button type="info" size="small" @click="toAddHandlar">添加</el-button>
    <el-button type="danger" size="small">删除</el-button>
    <el-table :data="customers">
        <el-table-column prop="id" label="编号"></el-table-column>
        <el-table-column prop="username" label="姓名"></el-table-column>
        <el-table-column prop="realname" label=" 真實姓名"></el-table-column>
        <el-table-column prop="username" label="姓名"></el-table-column>
        <el-table-column prop="vxid" label="微信"></el-table-column>
        <el-table-column prop="qq" label="qq"></el-table-column>
        <el-table-column prop="type" label="类型"></el-table-column>
        <el-table-column prop="idcard" label="身份证号" width="200"></el-table-column>
        <el-table-column prop="bankcard" label="银行卡号" width="200"></el-table-column>
        <!-- <el-table-column prop="gender" label="性别"></el-table-column> -->
        <el-table-column prop="telephone" label="联系方式"></el-table-column>
        <el-table-column label="操作">
            <template v-slot="slot">
              <!-- {{slot.row}} -->
                <a href="" @click.prevent="toDeleteHandle(slot.row.id)">删除</a>
                <a href="" @click.prevent="toupdateHandler(slot.row)">修改</a>
            </template>
        </el-table-column>
    </el-table>
    <!-- 分页 -->
    <el-pagination
    background
    layout="prev, pager, next"
    :total="50">
    </el-pagination>
    <!-- 分页结束 -->
    <el-dialog
        title="录入顾客信息"
        :visible.sync="visible"
        width="60%">
        {{form}}
        <!-- model双向数据绑定 -->
        <el-form :model="form" label-width="80px">
          <el-form-item label="用户名" >
            <el-input v-model="form.username"></el-input>
          </el-form-item>
          <el-form-item label="密码" >
            <el-input type="password" v-model="form.upassword"></el-input>  
          </el-form-item>
          <el-form-item label="telephone" >
            <el-input v-model="form.telephone"></el-input>  
          </el-form-item>
          <el-form-item label="qq" >
            <el-input v-model="form.qq"></el-input>  
          </el-form-item>
          <el-form-item label="身份证号" >
            <el-input v-model="form.idcard"></el-input>  
          </el-form-item>
          <el-form-item label="银行卡号" >
            <el-input v-model="form.bankcard"></el-input>  
          </el-form-item>       
        </el-form>
        <!-- :before-close="handleClose" -->
        <!-- <span>这是一段信息</span> -->
        <span slot="footer" class="dialog-footer">
            <el-button @click="closeModelHandler">取消</el-button>
            <el-button type="primary" @click="submitHandle">确定</el-button>
        </span>
    </el-dialog>
  </div>
</template>

<script>
import request from '@/utils/request'
import querystring from 'querystring'
export default {
  methods: {
    loadDate(){
      let url = 'http://localhost:6677/customer/findAll'
      request.get(url).then((response)=>{
        this.customers = response.data;
      })
    },
    submitHandle() {
      //通过request与后台进行交互
      let url = 'http://localhost:6677/customer/saveOrUpdate'
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
        let url = 'http://localhost:6677/customer/deleteById?id='+id;
        request.get(url).then((response)=>{
          this.loadDate();
          this.$message({
            type: 'success',
            message: response.message
          })
        })
      })
    },
    toupdateHandler(row) {
      this.form=row
      this.visible = true
    },
    closeModelHandler() {
      this.visible = false
    },
    toAddHandlar() {
      this.form ={
        type : "customer"
      }
      this.visible = true
    }
  },
  data() {
    return {
      visible: false,
       form:{
         type : "customer"
  },
      customers: [{
        // id: 1,
        // name: '张艺谋',
        // gender: '男',
        // telephone: '1234456778'
      }, {
        id: 2,
        name: '张三',
        gender: '男',
        telephone: '1234124778'
      }]
    }
  },
  created() {
    // // vue实例创建完毕
    // let url = 'http://localhost:6677/customer/findAll'
    // request.get(url).then((response)=>{
    //   // 将查询结果设置到customers
    //   this.customers = response.data;
    // },
    this.loadDate();
  },
 
}
</script>
<style scoped>

</style>
